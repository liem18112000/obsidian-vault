---
ai_hash: aac4b42cd1793618
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities: []
source: session 2026-08-13
status: seedling
tags:
- gke
- cloud-monitoring
- kubernetes
- pvc
- rbac
- gcloud
title: Read GKE PVC/volume peak usage from Cloud Monitoring when pod RBAC is denied
type: howto
---

# Read GKE PVC/volume peak usage from Cloud Monitoring when pod RBAC is denied

When pod RBAC blocks `kubectl exec`/`get` on a cluster (common on prod), you can still read a PVC/volume peak usage from **Cloud Monitoring**, which is gated by *project-level* IAM (Monitoring Viewer) rather than in-cluster RBAC — a different, often-available permission path.

**Metrics** (`k8s_pod` resource): `kubernetes.io/pod/volume/used_bytes` and `.../total_bytes`. Useful labels: `resource.labels.cluster_name`, `resource.labels.pod_name`, `metric.labels.volume_name`.

**Get the peak** by aligning with `ALIGN_MAX` over daily buckets, then taking the max across buckets client-side:

```bash
TOKEN=$(gcloud auth print-access-token)   # do NOT run under MSYS_NO_PATHCONV=1 (breaks gcloud on Windows)
curl -s -G "https://monitoring.googleapis.com/v3/projects/$PROJECT/timeSeries" \
  -H "Authorization: Bearer $TOKEN" \
  --data-urlencode 'filter=metric.type="kubernetes.io/pod/volume/used_bytes" AND resource.labels.cluster_name="'$CLUSTER'" AND resource.labels.pod_name=starts_with("mypod") AND metric.labels.volume_name="myvol"' \
  --data-urlencode "interval.startTime=$START" --data-urlencode "interval.endTime=$END" \
  --data-urlencode "aggregation.alignmentPeriod=86400s" \
  --data-urlencode "aggregation.perSeriesAligner=ALIGN_MAX"
```

**Gotchas learned:**
- `volume_name` is the **pod-spec volume name** (e.g. `temp-storage` / `tmp-scratch`), NOT the PVC name and NOT the mount path. If the filter returns `{"unit":"By"}` with zero series, drop the `volume_name` filter and list what labels actually exist.
- `--data-urlencode` values are safe from MSYS path-mangling because they start with `filter=` (not `/`), so you do NOT need `MSYS_NO_PATHCONV` here — and setting it would break the `gcloud` call that fetches the token.
- Time range: GKE system metrics retain ~6 weeks, so a 42-day window is the practical max for peak-finding.

Observed 2026-08-13 finding luz-docs-import scratch peak on klara-prod without pod exec access.

## Related

- [[Git Bash mangles Unix path args to kubectl exec — disable with MSYS_NO_PATHCONV]]
- [[Kubernetes]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import prod scratch peaks under 1GB — 100Gi tmp-scratch (LUZ-158230) is over-provisioned]]
- [[Git Bash mangles Unix path args to kubectl exec — disable with MSYS_NO_PATHCONV]]
- [[klara-prod is a separate GCP project, not a namespace]]
- [[Vertex AI Claude usage query - klara-nonprod]]
- [[Verify kubectl context before GKE rollout - _context file can disagree]]

%% ai-graph-end %%