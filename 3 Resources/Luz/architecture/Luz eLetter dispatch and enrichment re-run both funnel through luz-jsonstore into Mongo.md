---
title: "Luz eLetter dispatch and enrichment re-run both funnel through luz-jsonstore into Mongo"
created: 2026-09-09
type: observation
status: seedling
source: "PROD investigation 2026-09-09"
tags: [luz, architecture, luz-jsonstore, luz-docs, luz-eletter, request-flow]
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
