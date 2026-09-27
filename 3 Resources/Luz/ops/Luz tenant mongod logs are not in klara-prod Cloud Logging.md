---
ai_hash: fa267e267b2398f7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: PROD investigation 2026-09-09
status: seedling
tags:
- luz
- klara-prod
- cloud-logging
- mongodb
- gcloud
title: Luz tenant mongod logs are not in klara-prod Cloud Logging
type: observation
---

# Luz tenant mongod logs are not in klara-prod Cloud Logging

When investigating Luz PROD from a workstation: `gcloud` project **klara-prod** and kube context **gke_klara-prod_europe-west6-a_klara-prod** are present and queryable **read-only** (logging.viewer works). Luz application/service container logs live in the k8s namespace **prod** (`resource.type="k8s_container"`, `resource.labels.project_id="klara-prod"`).

**Gotcha:** the Luz **tenant mongod logs are NOT shipped to klara-prod Cloud Logging.** A search for `pod_name=~"mongo"` returns only the unrelated secmail cluster (`epost-zone-mta-mongodb-*` in namespace `prod-secmail`). So mongod-side evidence (slow-query log, `db.currentOp()`, whether pods were `OOMKilled`) cannot be read from Cloud Logging — you must go to the mongodb cluster directly (port-forward / cluster console). Until then, the app-side `MongoTimeoutException` storm + `time-consuming` latencies are the proxy for mongod collapse.

Also: PROD log payload timestamps are **CEST = UTC+2** (Cloud Logging `timestamp` is UTC; the log line text shows `+0200`).

## Related
[[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
[[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]

## Related

- [[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
- [[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
- [[Luz performance env cluster topology]]
- [[klara-prod is a separate GCP project, not a namespace]]
- [[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]
- [[MongoTimeoutException from UnknownHostException is a DNS fault, not DB overload]]

%% ai-graph-end %%