---
title: "LUZ-158230 import stores healthData as-is: no validation, no SNOMED label resolution, schemaless Mongo"
created: 2026-09-15
aliases: ["luz_docs_import import stores healthData as-is"]
type: lesson
status: seedling
source: "testing-agent run run-87933300, product-owner answers 2026-09-15"
tags: [luz, zip-import, healthData, snomed, test-oracle, LUZ-158230]
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
