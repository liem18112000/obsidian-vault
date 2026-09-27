---
ai_hash: 34ab931504de3501
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities: []
source: LUZ-158230 interrogation, session 2026-09-14
status: seedling
tags:
- luz
- ePost
- eArchive
- health-documents
- SNOMED
- LUZ-158230
title: HEALTH document type carries verbatim SNOMED healthData
type: concept
---

# HEALTH document type carries verbatim SNOMED healthData

The ZIP import introduces a **HEALTH** document type carrying structured **healthData**: a SNOMED-coded **category / facility / practice-setting**, each with **de/fr/it/en** labels.

Key decisions:
- The four-language labels are supplied **verbatim by the sender** in `metadata.json` and **trusted as-is** — no validation against a terminology/master-data service, and no local code->label table. An unknown code or a missing translation is **stored as-is, not rejected**.
- `healthData` is persisted as an **optional structured extension field on the existing eArchive document model** (a typed-metadata payload keyed by documentType). HEALTH is a **document subtype**, not a parallel store — eArchive read/list/search contracts stay stable.
- **No special HEALTH handling** this release: no consent gate, no restricted visibility, no distinct UI — HEALTH docs display like standard documents; only the internal structured metadata differs.

Established for LUZ-158230.

## Related
[[ePost eArchive ZIP import transfer.zip shape and luz_docs_import|ePost eArchive ZIP import: transfer.zip shape and luz_docs_import]]
[[luz_docs_import scope no sender auth, individual tenants, partial-import policy|luz_docs_import scope: no sender auth, individual tenants, partial-import policy]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[LUZ-158230 import stores healthData as-is no validation, no SNOMED label resolution, schemaless Mongo]]
- [[HEALTH documentType is generic passthrough; SNOMED metadata persisted verbatim]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]

%% ai-graph-end %%