---
ai_hash: e3f52872c9335d91
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities:
- luz-docs-import
- importZipName
- uploaded multipart filename
- on-disk zip filename
- DocsImportService
- FileMultipartUtil
- IdempotentImportService
- temp zip
- import job
- deduplication
- client
- test harness
- payloads
- idempotency key
- luz-docs-import AV scan covers only the metadata sidecar, never the document binary
source: session 2026-08-13
status: seedling
tags:
- luz-docs
- idempotency
- import
- multipart
- testing
title: luz-docs-import importZipName comes from the uploaded multipart filename, not
  the on-disk zip
type: observation
---

# luz-docs-import importZipName comes from the uploaded multipart filename, not the on-disk zip

In `luz_docs_import`, the idempotency key `importZipName` is derived from the **uploaded multipart part filename**, not from the fixture file name on disk.

Chain: `DocsImportService.saveReferenceFileToTemp` takes `FileMultipartUtil.getFileName(inputPart)` (the multipart part's `filename=`), writes the temp zip under that name, and `importZipFile` sets `importJob.setImportZipName(FilenameUtils.removeExtension(tmpZipFile.getName()))`. `IdempotentImportService` then dedupes on `importZipName` + each file's relative path.

**Consequences for testing re-import / dedupe:**
- To make a second upload be **skipped**, it must be POSTed with the **same multipart `filename`** as the first — even if the on-disk fixtures differ. (Fixture `34-edge-reimport-changed-body.zip` must be uploaded *as* `15-edge-duplicates-upload-twice.zip`; the two `36*` collision sets must both be uploaded *as* e.g. `documents.zip`.)
- Renaming the upload (same bytes, new `filename`) makes it a **different** `importZipName` → not deduped → re-imported (`35-edge-reimport-renamed-zip.zip`).
- Dedupe is path-based within an `importZipName`; content/size are ignored (see [[luz-docs-import AV scan covers only the metadata sidecar, never the document binary]] repo).

**Lesson:** when a dedupe/idempotency key is sourced from a client-supplied field (the multipart filename), the *client controls the key* — a test harness sets it via the upload, and two genuinely different payloads sent under one filename will cross-dedupe.

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_import ZIP import is path-based idempotent per importZipName]]
- [[luz-docs-import dedup identity is the uploaded zip filename (importZipName)]]
- [[ePost ZIP import dedup documents by job-success path, folders via view-controller]]
- [[luz_docs_import dedup folders via view-controller API, documents via import job history]]
- [[luz-docs-import createDocument is not idempotent — server-generated id, @Retry can duplicate on lost response]]

**Relations:**
- luz-docs-import — *uses* — importZipName
- importZipName — *is a type of* — idempotency key
- importZipName — *is derived from* — uploaded multipart filename
- importZipName — *is not derived from* — on-disk zip filename
- DocsImportService — *uses* — FileMultipartUtil
- FileMultipartUtil — *extracts* — uploaded multipart filename
- DocsImportService — *writes* — temp zip
- temp zip — *is named by* — uploaded multipart filename
- import job — *sets* — importZipName
- importZipName — *is set from* — temp zip
- IdempotentImportService — *performs* — deduplication
- deduplication — *is based on* — importZipName
- deduplication — *is based on* — each file's relative path
- deduplication — *ignores* — content
- deduplication — *ignores* — size
- uploaded multipart filename — *determines* — importZipName
- client — *controls* — idempotency key
- test harness — *sets* — uploaded multipart filename
- same multipart filename — *enables* — deduplication
- different multipart filename — *causes* — re-import
- payloads — *can cross-dedupe via* — same multipart filename
- luz-docs-import AV scan covers only the metadata sidecar, never the document binary — *is a related repo to* — luz-docs-import

%% ai-graph-end %%