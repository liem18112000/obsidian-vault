---
title: "HEALTH documentType is generic passthrough; SNOMED metadata persisted verbatim"
created: 2026-09-18
type: lesson
status: seedling
source: "Testing-Agent run-8eafe7ea refine, 2026-09-18"
tags: [luz-docs-import, LUZ-158230, LUZ-158241, ehealth, snomed]
---

# HEALTH documentType is generic passthrough; SNOMED metadata persisted verbatim

In `luz_docs_import`, `documentTypes` is an **array** carried through to **luz-docs**. The new **HEALTH** type is **not** a first-class, schema-locked type inside the importer — it is **generic passthrough**, and **luz-docs domain logic** judges validity.

SNOMED codes + de/fr/it/en labels for `documentCategory` / `facility` / `practiceSetting` / `author` are persisted **VERBATIM** as supplied by the sender — **no** cross-check against a reference value set inside this service. Reference value-set validation is **LUZ-158241s** scope, not this tickets.

## Related
[[luz_docs_import health ZIP import uses a two-layer failure model]]
