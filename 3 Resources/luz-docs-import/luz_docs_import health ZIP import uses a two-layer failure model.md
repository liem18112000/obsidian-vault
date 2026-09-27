---
title: "luz_docs_import health ZIP import uses a two-layer failure model"
created: 2026-09-18
type: lesson
status: seedling
source: "Testing-Agent run-8eafe7ea refine, 2026-09-18"
tags: [luz-docs-import, LUZ-158230, ehealth, import, gotcha]
---

# luz_docs_import health ZIP import uses a two-layer failure model

luz_docs_import handles per-document import failures in **two distinct layers**, and they behave oppositely:

1. **Import / ZIP-parse layer** — a **missing or malformed** `metadata.json`, or an **unrecognized `documentTypes`** value, does **not** fail the file. The binary document is still imported as a **SUCCESS**, using **default metadata**. Structural metadata problems degrade gracefully.
2. **luz-docs domain layer** — metadata that **is present but invalid** per luz-docs domain rules (a bad `documentType` value, malformed `healthData`) marks **that individual file** as a **FAILED** import — file by file.

Net: best-effort per-document, **never** atomic whole-ZIP. Absence of metadata ≠ failure; presence of *invalid* metadata = per-file failure. This corrected the Testing-Agent recommendation (which had proposed skip-and-log for malformed metadata).

## Related
[[senderTenantId is out of scope in luz_docs_import health ZIP import]]
[[HEALTH documentType is generic passthrough; SNOMED metadata persisted verbatim]]
