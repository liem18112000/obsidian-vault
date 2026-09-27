---
title: "senderTenantId is out of scope in luz_docs_import health ZIP import"
created: 2026-09-18
type: lesson
status: seedling
source: "Testing-Agent run-8eafe7ea refine, 2026-09-18"
tags: [luz-docs-import, LUZ-158230, ehealth, scope]
---

# senderTenantId is out of scope in luz_docs_import health ZIP import

`senderTenantId` / `senderCompanyId` are **out of scope** for `luz_docs_import` in the health ZIP import flow:

- The service does **not** validate them.
- The service does **not** use them for sender→recipient resolution or routing.
- They are **attribution-only** (display / provenance).

Recipient identity travels via the **existing authenticated upload/transport channel**, not via `metadata.json`. Consequence for testing: **sender-identity validation is NOT part of the import "done" acceptance criteria** — do not assert on it.

## Related
[[luz_docs_import health ZIP import uses a two-layer failure model]]
