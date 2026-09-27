---
ai_hash: 60378b7ccb7f0ef3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-18
entities: []
source: Testing-Agent run-8eafe7ea refine, 2026-09-18
status: seedling
tags:
- luz-docs-import
- LUZ-158230
- LUZ-158241
- ehealth
- snomed
title: HEALTH documentType is generic passthrough; SNOMED metadata persisted verbatim
type: lesson
---

# HEALTH documentType is generic passthrough; SNOMED metadata persisted verbatim

In `luz_docs_import`, `documentTypes` is an **array** carried through to **luz-docs**. The new **HEALTH** type is **not** a first-class, schema-locked type inside the importer — it is **generic passthrough**, and **luz-docs domain logic** judges validity.

SNOMED codes + de/fr/it/en labels for `documentCategory` / `facility` / `practiceSetting` / `author` are persisted **VERBATIM** as supplied by the sender — **no** cross-check against a reference value set inside this service. Reference value-set validation is **LUZ-158241s** scope, not this tickets.

## Related
[[luz_docs_import health ZIP import uses a two-layer failure model]]

%% ai-graph-start %%

**Related notes:**
- [[HEALTH document type carries verbatim SNOMED healthData]]
- [[luz_docs_import health ZIP import uses a two-layer failure model]]
- [[LUZ-158230 import stores healthData as-is no validation, no SNOMED label resolution, schemaless Mongo]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[luz_docs_import scope no sender auth, individual tenants, partial-import policy]]

%% ai-graph-end %%