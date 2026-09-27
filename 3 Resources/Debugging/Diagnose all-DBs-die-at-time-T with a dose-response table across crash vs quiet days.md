---
title: "Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days"
created: 2026-09-09
type: howto
status: seedling
source: "PROD investigation 2026-09-09"
tags: [debugging, observability, cloud-logging, mongodb, root-cause]
---

# Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days

When a datastore "dies at time T with no request spike," the fastest path to root cause is to **correlate the failure signature with a fan-out BATCH driver and prove it with a dose–response table across crash vs quiet days** — all from app-side Cloud Logging, without needing the DB server logs.

Method:
1. **Confirm the failure window** app-side: count `MongoTimeoutException` (or equivalent) per minute; find the peak minute. Pull the slowest ops (`time-consuming=` in access logs) just before it.
2. **Read the signature.** If *every* operation type is slow at once (e.g. 90–109 s across reads, writes, sorts alike), the DB is **saturated/thrashing**, not blocked on one query. A single heavy query slows one collection; saturation slows all of them.
3. **Find the fan-out driver.** Aggregate requests by tenant/DB and by originating service. A batch that touches hundreds of tenants at once is the "almost all DBs" culprit. Trace upstream (service → service) to the job that emits a distinctive start line (e.g. `"Starting re-run enricher for tenant: X"`).
4. **Prove causality with dose–response.** For several days, count the drivers job-start log line in the morning window and put it next to the timeout count. Crash days should show a high driver count, quiet days a low one. Monotonic relationship = strong causal evidence (far better than a single-day correlation).
5. **Distinguish trigger from after-effect.** A request *flood right after* the peak is usually the recovery backlog draining, not the cause — check whether it precedes or follows the timeout peak.

gcloud pattern used: `gcloud logging read resource.type="k8s_container" AND resource.labels.pod_name=~"X" AND textPayload:"marker" AND timestamp >= ... --format=value(...)` then pipe to `grep -oE | sort | uniq -c`.

## Related
[[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]

## Related

- [[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
