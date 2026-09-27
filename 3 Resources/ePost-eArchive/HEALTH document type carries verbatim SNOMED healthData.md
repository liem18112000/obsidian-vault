---
title: "HEALTH document type carries verbatim SNOMED healthData"
created: 2026-09-14
type: concept
status: seedling
source: "LUZ-158230 interrogation, session 2026-09-14"
tags: [luz, ePost, eArchive, health-documents, SNOMED, LUZ-158230]
---

# HEALTH document type carries verbatim SNOMED healthData

The ZIP import introduces a **HEALTH** document type carrying structured **healthData**: a SNOMED-coded **category / facility / practice-setting**, each with **de/fr/it/en** labels.

Key decisions:
- The four-language labels are supplied **verbatim by the sender** in `metadata.json` and **trusted as-is** — no validation against a terminology/master-data service, and no local code->label table. An unknown code or a missing translation is **stored as-is, not rejected**.
- `healthData` is persisted as an **optional structured extension field on the existing eArchive document model** (a typed-metadata payload keyed by documentType). HEALTH is a **document subtype**, not a parallel store — eArchive read/list/search contracts stay stable.
- **No special HEALTH handling** this release: no consent gate, no restricted visibility, no distinct UI — HEALTH docs display like standard documents; only the internal structured metadata differs.

Established for LUZ-158230.

## Related
[[ePost eArchive ZIP import: transfer.zip shape and luz_docs_import]]
[[luz_docs_import scope: no sender auth, individual tenants, partial-import policy]]
