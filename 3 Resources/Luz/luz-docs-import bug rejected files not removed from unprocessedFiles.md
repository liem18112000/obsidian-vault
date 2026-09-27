---
ai_hash: a8873e500ec0c68d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities: []
source: session 2026-08-10 (transfer.zip test run)
status: seedling
tags:
- luz
- luz-docs-import
- bug
- import
- gotcha
title: 'luz-docs-import bug: rejected files not removed from unprocessedFiles'
type: lesson
---

# luz-docs-import bug: rejected files not removed from unprocessedFiles

In luz-docs-import, `ImportJob.unprocessedFiles` must contain only files that reached NO terminal decision. It is seeded with the full non-metadata candidate set at `DocsImportAsyncService.createFolderAndDocuments` (~line 121), and every terminal branch removes the file from it: success (`createNewDocuments`, ~line 150), failure (`JobProgressWriter.recordFailure`, line 46), skipped/dedup (`JobProgressWriter.recordSkippedAll`, line 61).

**BUG:** the unsupported-file-type rejection branch in `processDocumentFile` (~lines 170-177) adds to `rejectedFiles` and `return`s WITHOUT `job.getUnprocessedFiles().remove(filePath)`. So rejected files (e.g. .csv/.exe/nested .zip) wrongly linger in `unprocessedFiles` — appearing in BOTH `rejectedFiles` and `unprocessedFiles`.

**Tell:** if `rejected=N` but `unprocessedFiles` has fewer than N entries, the difference is orphan JSON-metadata rejects — metadata files are excluded from `unprocessedFiles` at seed time (`!JsonUtils.isJsonMetadataFile`), so only the non-metadata file-type rejects leak.

**Fix:** add `job.getUnprocessedFiles().remove(filePath)` in the file-type-reject branch (line ~173), same as the other terminal paths. Found via the luz-docs-import-api-test skill run on transfer.zip. Related: [[Luz docs-import zip flow: upload-zip returns job-id, poll GET until DONE]].

## Related

- [[Luz docs-import zip flow: upload-zip returns job-id]]
- [[poll GET until DONE]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_import ZIP uploads are not virus-scanned (AntiviusScanningService is never wired in)]]
- [[Run volume import fixtures last; retry-exhaustion is transient saturation not a defect]]
- [[luz-docs-import JobProgressWriter checkpoints are for crash-durability + heartbeat, not UI progress]]
- [[luz-docs-import AV scan covers only the metadata sidecar, never the document binary]]
- [[DELETE import-jobs id has no live consumer]]

%% ai-graph-end %%