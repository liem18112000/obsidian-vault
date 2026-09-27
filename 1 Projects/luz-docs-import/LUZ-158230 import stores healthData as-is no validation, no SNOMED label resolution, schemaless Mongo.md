---
ai_hash: d061571c298bf50a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
aliases:
- luz_docs_import import stores healthData as-is
created: 2026-09-15
entities:
- LUZ-158230
- healthData
- validation
- SNOMED label resolution
- MongoDB
- ePost eArchive transfer.zip document-import feature
- product owner
- verbatim pass-through
- server-side schema validation
- metadata.json
- HEALTH document-type model
- terminology lookup service
- eArchive recipient-resolution API
- eArchive folder-management API calls
- Folder structure
- same-name recipient folders
- Partial-import semantics
- eArchive arrival notification
- Multi-sender validation
- Post Health
- Release
- LUZ-158243
- LUZ-159672
- Confluence spec
- Test-oracle
- tool's default recommendations
source: testing-agent run run-87933300, product-owner answers 2026-09-15
status: seedling
tags:
- luz
- zip-import
- healthData
- snomed
- test-oracle
- LUZ-158230
title: 'LUZ-158230 import stores healthData as-is: no validation, no SNOMED label
  resolution, schemaless Mongo'
type: lesson
---

# LUZ-158230 import stores healthData as-is: no validation, no SNOMED label resolution, schemaless Mongo

For the LUZ-158230 ZIP-import feature, the product owner confirmed the import is a **verbatim pass-through**, not a validating/enriching pipeline:

- **No server-side schema validation** of `metadata.json` / `healthData`.
- `healthData` is stored **as-is as schemaless MongoDB document data** — there is **no first-class `HEALTH` document-type model** and no generic document-type extension mechanism.
- **No SNOMED code→label resolution** at import *or* read — codes are stored and displayed verbatim (no terminology lookup service).

Surrounding integration decisions:
- Recipient eArchive/mailbox resolved via the **existing eArchive recipient-resolution API**.
- Folder structure preserved via **explicit eArchive folder-management API calls** that **merge into existing same-name recipient folders idempotently** (create-or-find, then attach documents) — not path-in-payload.
- **Partial-import** semantics: valid documents land even if siblings fail; failures are reported.
- Existing eArchive **arrival notification** triggered per landed batch.
- **Multi-sender** validation required (not just Post Health).
- Release is **gated** on LUZ-158243 + LUZ-159672 + the linked Confluence spec being complete/deployed.

Test-oracle consequence: assert data is stored *verbatim* and *unresolved*, not that it is validated or enriched. This overrides the tool's default recommendations, which leaned toward validation/resolution.

## Related
[[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]

## Related

- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert]]
- [[HEALTH document type carries verbatim SNOMED healthData]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]

**Relations:**
- LUZ-158230 — *is a* — ePost eArchive transfer.zip document-import feature
- LUZ-158230 — *stores* — healthData
- LUZ-158230 — *does not include* — validation
- LUZ-158230 — *does not include* — SNOMED label resolution
- healthData — *is stored in* — MongoDB
- product owner — *confirmed* — verbatim pass-through
- verbatim pass-through — *applies to* — LUZ-158230
- LUZ-158230 — *does not perform* — server-side schema validation
- server-side schema validation — *applies to* — metadata.json
- server-side schema validation — *applies to* — healthData
- LUZ-158230 — *lacks* — HEALTH document-type model
- LUZ-158230 — *does not use* — terminology lookup service
- ePost eArchive transfer.zip document-import feature — *resolves recipients via* — eArchive recipient-resolution API
- ePost eArchive transfer.zip document-import feature — *preserves* — Folder structure
- Folder structure — *preserved via* — eArchive folder-management API calls
- eArchive folder-management API calls — *merge into* — same-name recipient folders
- LUZ-158230 — *has* — Partial-import semantics
- ePost eArchive transfer.zip document-import feature — *triggers* — eArchive arrival notification
- LUZ-158230 — *requires* — Multi-sender validation
- Multi-sender validation — *is not limited to* — Post Health
- Release — *gated by* — LUZ-158243
- Release — *gated by* — LUZ-159672
- Release — *gated by* — Confluence spec
- Test-oracle — *asserts* — verbatim pass-through
- Test-oracle — *overrides* — tool's default recommendations

%% ai-graph-end %%