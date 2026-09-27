---
title: "LUZ-158230 test approach: full-chain real-deps integration with fully-materialized done"
created: 2026-09-18
type: lesson
status: seedling
source: "Testing-Agent run-8eafe7ea refine, 2026-09-18"
tags: [luz-docs-import, LUZ-158230, testing, integration-test]
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
