---
title: "GCP Alert Policy Creation Script - Manual Documentation"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48803184687/GCP+Alert+Policy+Creation+Script+-+Manual+Documentation
space: "LUZ"
topic: infra
relevance: 0.757
depth: 2.84
updated: 2025-10-29
attachments: 0
tags:
  - confluence
  - infra
  - space/luz
---

# GCP Alert Policy Creation Script - Manual Documentation

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-10-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48803184687/GCP+Alert+Policy+Creation+Script+-+Manual+Documentation)
> Relevance 0.757 · topic `infra`

### **File**

`cluster/gcp/manual_script/create_gcp_alert_policy.sh`

------------------------------------------------------------------------

## **1. Purpose**

This script automates the **creation of monitoring metrics and alert policies** in **Google Cloud Monitoring (Stackdriver)** for the Luz microservices ecosystem.  
It ensures consistent setup of:

- Log-based custom metrics (for REST client calls, latency, errors, and OOMs)

- Resource usage alerts (CPU and Memory utilization)

- Error rate alerts (\>1%)

- Slow response rate alerts (\>1%)

- Out-of-Memory (OOM) detection alerts

This eliminates the need for manual policy creation via the GCP console and guarantees all services follow the same alerting thresholds and naming conventions.

------------------------------------------------------------------------

## **2. Usage**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="adbb1407-093f-43d0-8ea5-71797dd18828" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./create_gcp_alert_policy.sh <LUZ_GCP_PROJECT_ID> <ENV>
```

</div>

</div>

### **Example**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c200c98a-8f41-4792-8f0c-c60cb52fb8de" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./create_gcp_alert_policy.sh klara-performance performance
```

</div>

</div>

### **Arguments**

<div>

|  |  |
|----|----|
| Parameter | Description |
| `<LUZ_GCP_PROJECT_ID>` | The GCP project ID containing the Luz workloads |
| `<ENV>` | The environment namespace (e.g., `performance`, `staging`, `prod`) |

</div>

------------------------------------------------------------------------

## **3. Script Overview**

### **Step-by-step workflow**

#### **3.1 Environment Initialization**

- Validates arguments and loads environment variables from `configuration/env.sh`.

- Detects and ensures the script runs in the correct working directory.

#### **3.2 Notification Channel Management**

- Reads the email list from `$LUZ_DOCS_ALERT_EMAIL_CHANNEL`.

- Creates notification channels automatically via:

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7ffc86fe-99e7-43d6-9299-de5998cbfec7" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  gcloud alpha monitoring channels create --type=email
  ```

  </div>

  </div>

- Reuses existing channels if found.

#### **3.3 Core Functions**

##### **create_log_metric()**

Creates or verifies the existence of a **log-based metric** in GCP.

Example metric:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="23cdfe94-0678-4f39-bc66-37eb5f49041e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
performance-luz-docs-call-luz-jsonstore-rest-client-add-document-total-count
```

</div>

</div>

Filters logs using `gcloud logging metrics create` with filters like:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dfb886d1-90cd-43a0-bbc9-9c14b7fb9b30" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
labels.k8s-pod/app="luz-docs" AND textPayload:"luz-jsonstore:8080"
```

</div>

</div>

------------------------------------------------------------------------

##### **build_alert_policy_json()**

Generates a **JSON definition** for ratio-based alert policies (e.g., error rate, latency).

Parameters include:

- Metric for numerator (`error_count`)

- Metric for denominator (`total_count`)

- Threshold value

- Severity (`CRITICAL` or `WARNING`)

Automatically injects:

- Alert display name

- Documentation content

- Filter conditions

- Auto-close period: `86400s`

- Denominator filter (if applicable)

Example output snippet:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9abfeb14-400e-4e79-92a9-94813c20cc19" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
"conditionThreshold": {
  "filter": "resource.type=\"k8s_container\" AND metric.type=\"logging.googleapis.com/user/performance-luz-docs-call-luz-jsonstore-rest-client-add-document-error-count\"",
  "denominatorFilter": "resource.type=\"k8s_container\" AND metric.type=\"logging.googleapis.com/user/performance-luz-docs-call-luz-jsonstore-rest-client-add-document-total-count\"",
  "comparison": "COMPARISON_GT",
  "thresholdValue": 0.01,
  "duration": "900s"
}
```

</div>

</div>

------------------------------------------------------------------------

##### **build_alert_policy_json_for_resource_usage()**

Specialized version for **CPU and memory utilization alerts**.

- Uses GCP built-in metrics:

  - `kubernetes.io/container/cpu/limit_utilization`

  - `kubernetes.io/container/memory/limit_utilization`

- Removes `denominatorFilter` (since these are single-value metrics)

- Adds 5-minute rolling window (`duration=300s`)

- Aligns time series to `ALIGN_MEAN`

Example condition:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="972edf0b-5d92-4039-934f-8598a7e48a7c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
"filter": "resource.type=\"k8s_container\" AND resource.labels.namespace_name=\"performance\" AND resource.labels.container_name=\"luz-docs\" AND metric.type=\"kubernetes.io/container/memory/limit_utilization\""
```

</div>

</div>

------------------------------------------------------------------------

##### **create_alert_policy()**

Creates or skips an alert if it already exists:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5e48c6fb-883c-460d-91ed-faaf2ffeb907" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
gcloud alpha monitoring policies create \
  --project "$PROJECT_ID" \
  --policy-from-file=- \
  --notification-channels="$CHANNEL_ID_LIST"
```

</div>

</div>

The alert ID is extracted and displayed upon successful creation.

------------------------------------------------------------------------

##### **create_oom_log_metric()**

Creates log-based metrics to detect **OOMKilled** or **OutOfMemoryError** events:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9e3d7963-50a4-4b67-8758-29a537df95fb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
(textPayload:"OutOfMemoryError") OR (textPayload:"/OOMKilled") OR (textPayload:"exit code 137")
```

</div>

</div>

------------------------------------------------------------------------

## **4. Alert Types Created**

<div>

|  |  |  |  |
|----|----|----|----|
| Alert Type | Metric Source | Threshold | Description |
| **High Memory Usage** | `kubernetes.io/container/memory/limit_utilization` | \> 75% | Detects containers nearing memory limit |
| **High CPU Usage** | `kubernetes.io/container/cpu/limit_utilization` | \> 75% | Detects excessive CPU utilization |
| **Error Rate Alert** | log-based metrics | \> 1% | Non-2xx HTTP responses |
| **Slow Response Rate** | log-based metrics | \> 1% | Requests exceeding SLA response time |
| **OOM Alerts** | log-based metrics | \> 0 | Detects OOM events in pods |

</div>

------------------------------------------------------------------------

## **5. Metric Naming Convention**

All metrics follow a consistent naming structure:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f26056b0-026b-4329-8da9-43bca7b0083c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<NAMESPACE>-<SERVICE>-<TARGET>-<CATEGORY>-<SUFFIX>
```

</div>

</div>

Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3bb440b0-845a-443c-aa8a-61d0b7fd1ac0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
performance-luz-docs-call-luz-jsonstore-rest-client-add-document-total-count
performance-luz-docs-call-luz-jsonstore-rest-client-add-document-error-count
performance-luz-docs-call-luz-vault-rest-client-taking-long-time-count
performance-luz-docs-out-of-memory-total-count
```

</div>

</div>

------------------------------------------------------------------------

## **6. Alert Policy Naming Convention**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="66388e43-d758-477c-9f30-9036f6b07ca2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<NAMESPACE>-alert-<SERVICE>-<description>
```

</div>

</div>

Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="eb84ad07-6959-4868-8d25-4a2d2d5865a9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
performance-alert-luz-docs-using-more-cpu-than-75%
performance-alert-luz-jsonstore-rest-client-error-rate-bigger-than-1-percent
performance-alert-luz-docs-oom-detected
```

</div>

</div>

------------------------------------------------------------------------

## **7. Threshold Defaults**

<div>

|                    |            |        |          |
|--------------------|------------|--------|----------|
| Category           | Threshold  | Window | Severity |
| CPU / Memory       | 0.75 (75%) | 5 min  | WARNING  |
| Error rate         | 0.01 (1%)  | 15 min | WARNING  |
| Slow response rate | 0.01 (1%)  | 15 min | WARNING  |
| OOM occurrence     | \> 0       | 15 min | CRITICAL |

</div>

------------------------------------------------------------------------

## **8. Dependencies**

- `gcloud` CLI (alpha monitoring components)

- `jq` for JSON manipulation

- Valid GCP IAM permissions:

  - `roles/monitoring.editor`

  - `roles/logging.configWriter`

------------------------------------------------------------------------

## **9. Logging & Debugging**

You can inspect created resources:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d308498d-e6be-48d3-a25c-84e4853c19a2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
gcloud alpha monitoring policies list --project <PROJECT_ID> --format="table(displayName,name)"
gcloud logging metrics list --project <PROJECT_ID>"
```

</div>

</div>

To delete a specific alert:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d5989164-40f1-40ed-890d-72dda677da93" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
gcloud alpha monitoring policies delete <POLICY_ID> --project <PROJECT_ID>
```

</div>

</div>

------------------------------------------------------------------------

## **10. Common Errors**

<div>

|  |  |  |
|----|----|----|
| Error Message | Cause | Solution |
| `Cannot find metric(s) that match type = ...` | Metric not yet propagated | Wait 5–10 minutes, rerun script |
| `unexpected EOF` | Missing `EOF` in heredoc | Ensure proper indentation and quoting |
| `Invalid JSON` | jq transformation failed | Validate template JSON and input params |
| `Already exists` | Alert already present | Expected, script skips safely |

</div>

------------------------------------------------------------------------

## **11. Future Improvements**

✅ Add **auto-labeling** of alerts by team or service owner  
✅ Include **auto-removal** for stale alerts  
✅ Support for **Grafana-compatible output**  
✅ Option to **update** (not just create) alert policies if already exist

------------------------------------------------------------------------

## **12. Summary**

This script provides a **fully automated, consistent, and idempotent** setup for all GCP Monitoring alerts across Luz services.  
It simplifies onboarding of new microservices, ensures consistent thresholds, and reduces human error when creating or maintaining alerts manually.
