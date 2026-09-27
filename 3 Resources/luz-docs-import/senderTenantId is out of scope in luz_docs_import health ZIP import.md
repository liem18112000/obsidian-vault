---
ai_hash: 339d0fbb956b31a3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-18
entities: []
source: Testing-Agent run-8eafe7ea refine, 2026-09-18
status: seedling
tags:
- luz-docs-import
- LUZ-158230
- ehealth
- scope
title: senderTenantId is out of scope in luz_docs_import health ZIP import
type: lesson
---

# senderTenantId is out of scope in luz_docs_import health ZIP import

`senderTenantId` / `senderCompanyId` are **out of scope** for `luz_docs_import` in the health ZIP import flow:

- The service does **not** validate them.
- The service does **not** use them for sender→recipient resolution or routing.
- They are **attribution-only** (display / provenance).

Recipient identity travels via the **existing authenticated upload/transport channel**, not via `metadata.json`. Consequence for testing: **sender-identity validation is NOT part of the import "done" acceptance criteria** — do not assert on it.

## Related
[[luz_docs_import health ZIP import uses a two-layer failure model]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_import scope no sender auth, individual tenants, partial-import policy]]
- [[luz_docs_import health ZIP import uses a two-layer failure model]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()]]
- [[LUZ-158230 test approach full-chain real-deps integration with fully-materialized done]]

%% ai-graph-end %%