---
ai_hash: cc88931372f31e2b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.794
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47148040363/Monitoring+with+Managed+Prometheus
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: Monitoring with Managed Prometheus
topic: infra
type: source
updated: 2022-07-25
---

# Monitoring with Managed Prometheus

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-07-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47148040363/Monitoring+with+Managed+Prometheus)
> Relevance 0.794 · topic `infra`

## Setup

To enable Prometheus in your cluster run the following manual script from luz_kubernets:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="899c7f4a-8814-432f-8295-66e95cf126d3" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cluster/gcp/manual_script/enable_managed_prometheus.sh gcp-project environment
```

</div>

</div>

Then wait a couple minutes. Note that the cluster will not be responsive during this time (i.e. checking for its status or listing the workloads) but the pods keep running throughout.

<div id="expander-849415738" class="expand-container conf-macro output-block" hasbody="true" macro-id="f3d75c72-d846-4bf4-8edb-9decc2f03717" macro-name="expand">

<div id="expander-control-849415738" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Additional Info: Manual Setup</span>

</div>

<div id="expander-content-849415738" class="expand-content expand-hidden">

You could also enable prometheus manually by simply running the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0d0905de-0214-46f5-b083-90968ac68067" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
gcloud beta container clusters update $LUZ_GCP_CLUSTER_NAME \
  --enable-managed-prometheus --zone $LUZ_GCP_ZONE
```

</div>

</div>

The luz_kubernetes script just checks if prometheus is already enabled, and otherwise run the command above.

</div>

</div>

## Module configuration

### 1) Add metrics port

First you must expose the metrics of your wildfly up. To do so we use an nginx sidecar container which exposes only host:9990/metrics on port 9090. Add it to you deployment like this:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="834caf4c-77f1-4dea-a5c1-faed61fe33ef" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: apps/v1
kind: Deployment
metadata:
  name: your-app
  labels:
    klara.ch/module: your-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: your-app
  template:
    metadata:
      labels:
        app: your-app
    spec:
      containers:
      - name: your-app
        image: gcr.io/klara-repo/your-app:cbce6d9229b2443008363227f87bffe3204686d1
        [...]
      - name: metrics-proxy
        image: gcr.io/klara-repo/nginx-metrics-proxy:latest
        ports:
        - containerPort: 9090
          name: metrics
      [...]
```

</div>

</div>

The resource usage of the nginx sidecar is negligible. It uses less than 1 milliCPU and 3MB of RAM, even if metrics are requested every few milliseconds. In our real use case nginx needs to handle only a single connection every couple of seconds, therefore the performance and cost impact of the sidecar can be ignored.

### 2) Add a PodMonitoring resource

Additionally you must add a new kubernetes \`PodMonitoring\` resource, referencing the deployment and port from above:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a8ecd736-8c11-41ca-8a40-fbb15eab91fb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: monitoring.googleapis.com/v1
kind: PodMonitoring
metadata:
  name: your-app
  labels:
    app.kubernetes.io/name: your-app
spec:
  selector:
    matchLabels:
      app: your-app
  endpoints:
  - port: metrics
    interval: 15s
```

</div>

</div>

Note that the match labels and port name match the values from above. Reasonable values for the scraping interval range from 5s to a couple minutes.

### 3) \[Optional\] Enable additional wildfly metrics

Most metrics from wildfly are not collected out-of-the box. Because collecting them has a minimal performance impact you must enable them manually and with care. To do so change the startup command of your wildfly container by setting the `wildfly.statistics-enabled` system property to `true`.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0f8641cd-e71e-4282-8f5c-5ed524e067f2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: apps/v1
kind: Deployment
metadata:
  name: your-app
  labels:
    klara.ch/module: your-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: your-app
  template:
    metadata:
      labels:
        app: your-app
    spec:
      containers:
      - name: your-app
        image: gcr.io/klara-repo/your-app:cbce6d9229b2443008363227f87bffe3204686d1
        command:
          - /bin/bash
          - -c
          - "setup_max_ram_percentage.sh && /opt/jboss/wildfly/bin/standalone.sh -b 0.0.0.0 -Dwildfly.statistics-enabled=true"
      [...]
```

</div>

</div>

### 4) Accessing the metrics in Google Cloud Monitoring

The metrics collected by prometheus can be accessed in the clod montoring as any other metric. For example to check the pods heap usage use the following MQL query:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0db30090-1669-478b-93ac-180297cc78de" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
fetch prometheus_target
| metric 'prometheus.googleapis.com/base_memory_committedHeap_bytes/gauge'
| group_by 1m, [used_mean: mean(value.base_memory_committedHeap_bytes)]
| value [used_mean_div: div(used_mean, 1024 * 1024)]
| every 1m 
```

</div>

</div>

Here we fetch the `base_memory_committedHeap_bytes` metric from prometheus. The metric has type `gauge` so the full metric name is `prometheus.googleapis.com/base_memory_committedHeap_bytes/gauge`. We group the values per minute and then convert the byte values to megabytes by dividing it twice by 1024.

Here’s an example Dashboard showing the memory metrics available to prometheus:

<a href="https://console.cloud.google.com/monitoring/dashboards/builder/8372a0fe-6255-46dd-9c0a-8191d3ee6944?project=klara-nonprod&amp;dashboardBuilderState=%257B%2522editModeEnabled%2522:false%257D&amp;timeDomain=6h" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboards/builder/8372a0fe-6255-46dd-9c0a-8191d3ee6944?project=klara-nonprod&amp;dashboardBuilderState=%7B%22editModeEnabled%22:false%7D&amp;timeDomain=6h</a>

%% ai-graph-start %%

**Related notes:**
- [[14. Deploy openshift cluster monitoring]]
- [[luz-docs performance JVM thread metrics endpoint]]
- [[Axonivycloud - Monitoring EKS cluster using Prometheus and Grafana]]
- [[Apply branch code of webclient-nginx-ingress.yaml]]
- [[GCP Alert Policy Creation Script - Manual Documentation]]

%% ai-graph-end %%