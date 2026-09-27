---
ai_hash: f70eee3ca90bedb6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
entities: []
---

> [!warning] CORRECTED 2026-09-09 (cross-check vs reference artifact + PROD re-verification)
> The root cause is **DNS**, not MongoDB saturation. The `MongoTimeoutException` storm is caused by **`java.net.UnknownHostException`** resolving the mongod hostnames (`*.prod-mongodb-clusters.svc.cluster.local`) — 2,000+ in the crash window, ~all in luz-jsonstore — while **`coredns-custom` fails its liveness probe (HTTP 404) and is killed/restarted every few minutes**. The MongoDBs were UP; slowest real query ~2.4s. The 90-109s "latencies" were requests blocked on DNS/server-selection retries, NOT heavy queries. The enrichment/dose-response correlation is REAL but is an **amplifier hypothesis** (luz-jsonstore creates MongoClients per tenant → floods DNS), not DB overload. Do not assert OOM/saturation. See [[MongoTimeoutException from UnknownHostException is a DNS fault, not DB overload]].

---
title: "luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning"
created: 2026-09-09
type: observation
status: seedling
source: "PROD investigation 2026-09-09"
tags: [luz, mongodb, luz-docs, incident, enrichment, klara-prod]
---

# luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning

The recurring PROD incident where "almost all tenant MongoDBs get killed/OOM around 08:30–08:43 CEST" is driven by the **luz-docs document enrichment re-run campaign**, not by any single heavy query and not by a request spike.

`ch.klara.luz.docs.jobs.EnrichmentJob` (running in the `luz-docs-batch` StatefulSet) fires `executeEnrichmentJob → "Starting re-run enricher for tenant: X"` and fans out across **~800 tenants** each morning. Every re-run: queries `luz_docs_migration_campaign` (`sort last_updated_time desc`) in luz-jsonstore, downloads the file from GCS, runs enrichers (thumbnail, Analyze/OCR), then writes the document + `enrichmentstatus` + audit/fingerprint records back through luz-jsonstore into that tenants MongoDB. Multiplied across ~800 tenants this reads/writes into essentially every replica set at once → WiredTiger cache + connection exhaustion → 90–109 s latencies on ALL op types → mongods killed.

**Dose–response proof** (enrichment re-run starts 04–09 UTC vs luz-jsonstore MongoTimeoutExceptions):
- Sep 8: 12,004 → 4,933 timeouts (crash, peak 08:39 CEST; 7,978 of the re-runs land in the 08:00–09:00 CEST hour)
- Sep 1: 5,246 → 2,499 (crash, peak 08:04 CEST)
- Sep 5: 2,686 → 0 (healthy)
- Sep 6: 769 → 0 (healthy)

It is **intermittent / backlog-driven, not a fixed daily cron** — that is why it "happens quite often around 08:30+" rather than at an exact time. The `luz_docs_migration_campaign` request flood at 08:43 (2,300–2,600/min) is the **recovery backlog draining as the DBs come back**, i.e. the after-effect, not the trigger. luz-jsonstore here is the victim/amplifier (its own queries time out); it adds baseline pressure via its backup job.

Fix levers: throttle the campaign concurrency + find why re-runs spiked; index `last_updated_time`; give mongods headroom.

## Related
[[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]
[[luz-jsonstore backup double-scans every collection every 5 minutes]]
[[Luz tenant mongod logs are not in klara-prod Cloud Logging]]

## Related

- [[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]
- [[luz-jsonstore backup double-scans every collection every 5 minutes]]
- [[Luz tenant mongod logs are not in klara-prod Cloud Logging]]

%% ai-graph-start %%

**Related notes:**
- [[Luz eLetter dispatch and enrichment re-run both funnel through luz-jsonstore into Mongo]]
- [[Luz DNS storm gate is both CoreDNS replicas down together plus morning load, not enrichment volume]]
- [[MongoTimeoutException from UnknownHostException is a DNS fault, not DB overload]]
- [[Luz tenant mongod logs are not in klara-prod Cloud Logging]]
- [[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]

%% ai-graph-end %%