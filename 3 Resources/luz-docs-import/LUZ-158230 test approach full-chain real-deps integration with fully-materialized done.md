---
ai_hash: d3de9cdbeae383d0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-18
entities: []
source: Testing-Agent run-8eafe7ea refine, 2026-09-18
status: seedling
tags:
- luz-docs-import
- LUZ-158230
- testing
- integration-test
title: 'LUZ-158230 test approach: full-chain real-deps integration with fully-materialized
  done'
type: lesson
---

# LUZ-158230 test approach: full-chain real-deps integration with fully-materialized done

Test approach agreed for **LUZ-158230** ("[eArchive] Health documents ZIP import in ePost"):

- **Hit ALL real deps** — full chain `luz_docs_import → luz_antivirus → luz_docs → luz_jsonstore` (+ Mongo), **no stubs** — and assert the **genuine stored end-state**.
- **Coverage bar**: happy + negative + boundary + error.
- **"Done" = fully materialized** end-state: import job reaches **DONE** AND documents/folders/`healthData` are **persisted and readable** in eArchive — not merely "accepted".

Backdrop: eHealth SDK in the Post-App is shut down end of 2026; health docs must move to eArchive before then. `transfer.zip` = documents + folder structure + per-document `<file>.metadata.json` sidecar; OS artefacts (`.DS_Store`, `Thumbs.db`, `__MACOSX/`) ignored; folder structure reproduced 1:1. Mirrors the luz-docs-import-api-test skill (real REST upload + Mongo job/collection tracking).

## Related
[[luz_docs_import health ZIP import uses a two-layer failure model]]
[[senderTenantId is out of scope in luz_docs_import health ZIP import]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[LUZ-158230 eArchive Health ZIP import - golden test fixture matrix location]]
- [[LUZ-158230 import stores healthData as-is no validation, no SNOMED label resolution, schemaless Mongo]]
- [[luz_docs_import scope no sender auth, individual tenants, partial-import policy]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]

%% ai-graph-end %%