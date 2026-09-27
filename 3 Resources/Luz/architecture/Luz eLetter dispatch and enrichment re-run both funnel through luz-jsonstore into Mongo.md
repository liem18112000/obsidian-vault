---
ai_hash: 48709b030f8d6b76
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: PROD investigation 2026-09-09
status: seedling
tags:
- luz
- architecture
- luz-jsonstore
- luz-docs
- luz-eletter
- request-flow
title: Luz eLetter dispatch and enrichment re-run both funnel through luz-jsonstore
  into Mongo
type: observation
---

# Luz eLetter dispatch and enrichment re-run both funnel through luz-jsonstore into Mongo

Every PROD request that times out at the tenant mongods arrives through **luz-jsonstore** — it is the single data-access funnel. During the morning crash two independent streams converge on it (traced from PROD inter-service ClientResponseFilter logs, Sep 8):

READ stream (eLetter dispatch): `luz-eletter` + `luz-eletter-dispatcher` + `luz-eletter-large-dispatcher` -> `luz-docs-view-controller` (and `-batch`) -> `luz-docs` -> `luz-jsonstore` -> Mongo. Reads documents/folders/profile/security-classes to render+dispatch electronic letters. Dispatch calls to view-controller ramp 139 -> 2,410/min at 08:41 CEST (the crash minute).

WRITE stream (enrichment re-run): `luz-docs-batch` (EnrichmentJob) -> `luz-jsonstore` -> Mongo, plus off-DB `luz-antivirus`/`luz-thumbnail`/analyze(OCR)/GCS/vault. Writes documents + enrichmentstatus (+ auditlogs/lastFingerPrint). 7,978 re-run starts in the 08-09h CEST hour.

Shared: both lanes emit `luz_docs_migration_campaign?sort=last_updated_time desc` (~1,108) per doc/tenant, and both cache-miss to `luz-cache` (GET returns WARNING) -> fallback straight to Mongo. Because jsonstore is downstream of BOTH streams, it (and the mongods) absorbs the SUM — which is why they peak together and saturate rather than taking turns.

Edge map (who calls whom): view-controller -> luz-docs(938)+luztenant(421); luz-docs -> luz-cache(649)+jsonstore(639)+vault; luz-docs-batch -> jsonstore(616)+cache(531)+antivirus+thumbnail. eLetter -> view-controller(+batch). webclient -> view-controller (smaller, interactive).

## Related
[[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
[[Intermittent DB saturation = stacked loads crossing a fixed ceiling]]

## Related

- [[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
- [[Intermittent DB saturation = stacked loads crossing a fixed ceiling]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
- [[Luz DNS storm gate is both CoreDNS replicas down together plus morning load, not enrichment volume]]
- [[luz-jsonstore backup double-scans every collection every 5 minutes]]
- [[eArchive request flow and log correlation (perf)]]
- [[Diagnose all-DBs-die-at-time-T with a dose-response table across crash vs quiet days]]

%% ai-graph-end %%