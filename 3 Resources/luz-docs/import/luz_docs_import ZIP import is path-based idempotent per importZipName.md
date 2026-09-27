---
ai_hash: c8b1372f34689838
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-18
entities: []
source: session 2026-09-18 run-655b14d9
status: seedling
tags:
- luz-docs
- luz_docs_import
- zip-import
- idempotency
- gotcha
- LUZ-158230
title: luz_docs_import ZIP import is path-based idempotent per importZipName
type: lesson
---

# luz_docs_import ZIP import is path-based idempotent per importZipName

The luz_docs_import ZIP import deduplicates on the **relative entry path, scoped per `importZipName`** (via `IdempotentImportService`), recorded in the import job's tracking history — not on file content.

- Re-uploading a same-named ZIP: every already-recorded path lands in `skippedFiles` with `DETAIL_ALREADY_IMPORTED`.
- **Content-blind**: same path + different bytes is still skipped (the changed content is silently dropped).
- Renaming the ZIP defeats dedup: identical contents under a different `importZipName` re-import fully.
- Folders are always reused, never duplicated.

This is the *real* implemented behaviour and it differs from the BRD (LUZ-158230) prose, which describes `BRule-6` as skip-by-same-name-and-size. When designing tests, assert against the path/importZipName idempotency, and treat content-blind skip as a known limitation to surface, not a bug to fix.

Source: LUZ-158230 test run (run-655b14d9), fixtures 15/34/35/36; codegraph axonivy-prod/luz_docs_import.

## Related

- [[LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_import dedup folders via view-controller API, documents via import job history]]
- [[ePost ZIP import dedup documents by job-success path, folders via view-controller]]
- [[luz-docs-import importZipName comes from the uploaded multipart filename, not the on-disk zip]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
- [[luz_docs_import ZIP import behavior — limits, allow-list, idempotency, AV scope]]

%% ai-graph-end %%