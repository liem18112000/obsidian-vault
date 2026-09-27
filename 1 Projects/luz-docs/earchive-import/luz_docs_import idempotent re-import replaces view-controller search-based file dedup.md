---
ai_hash: 53c47ad5fe948aab
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-07
entities:
- luz_docs_import
- view-controller search-based file dedup
- idempotent re-import
- luz-docs-view-controller
- /documents/search
- SearchDocumentResponse
- SearchDocumentResult
- Files
- Reference
- QueryRequest
- Query
- Term
- Archive.isSameParentFolder
- JsonUtils.converObjectToJson
- LuzDocsViewControllerService.searchDocument
- JobProgressWriter.recordSkipped(File)
- JobProgressWriter.recordSkippedAll(List)
- ch.klara.luz.docsimport
- Java 17
- WildFly EJB
- json-store
- eArchive ZIP import
- Read-time non-persisted state repair is an anti-pattern; use a scheduled reconciler
- previous job's history
- name + byte size
source: session 2026-08-07 luz_docs_import
status: seedling
tags:
- luz-docs
- earchive
- dedup
- idempotency
- refactor
title: 'luz_docs_import: idempotent re-import replaces view-controller search-based
  file dedup'
type: argument
---

# luz_docs_import: idempotent re-import replaces view-controller search-based file dedup

**Decision:** In luz_docs_import, the per-file dedup that called luz-docs-view-controller's `/documents/search` (matching an existing document by **name + byte size** in the target folder before creating) was removed. **Idempotent re-import** — skipping files already recorded in a previous job's history — already covers the real 'don't import the same file twice' need.

**Why:** the search-based dedup cost a **network round-trip per file** during import, and pulled in a whole chain of DTOs that existed only to parse that response: `SearchDocumentResponse`, `SearchDocumentResult`, `Files`, `Reference`, `QueryRequest`, `Query`, `Term` — plus `Archive.isSameParentFolder`, `JsonUtils.converObjectToJson`, `LuzDocsViewControllerService.searchDocument` and its REST-client binding, and `JobProgressWriter.recordSkipped(File)`. All became dead code and were deleted.

**Gotcha to remember:** the two 'skip' paths are different — `recordSkippedAll(List)` is the idempotent re-import skip (kept); `recordSkipped(File)` was the search-dedup skip (removed). Don't conflate them.

Context: service ch.klara.luz.docsimport, Java 17 / WildFly EJB, json-store backed jobs; eArchive ZIP import. Same session also removed the read-time timeout flip — see [[Read-time non-persisted state repair is an anti-pattern; use a scheduled reconciler]].

## Related

- [[Read-time non-persisted state repair is an anti-pattern; use a scheduled reconciler]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import createDocument is not idempotent — server-generated id, @Retry can duplicate on lost response]]
- [[luz_docs_import dedup folders via view-controller API, documents via import job history]]
- [[ePost ZIP import dedup documents by job-success path, folders via view-controller]]
- [[luz_docs_import ZIP import is path-based idempotent per importZipName]]
- [[luz-docs-import JobProgressWriter checkpoints are for crash-durability + heartbeat, not UI progress]]

**Relations:**
- luz_docs_import — *replaces* — view-controller search-based file dedup
- luz_docs_import — *uses* — idempotent re-import
- view-controller search-based file dedup — *called* — luz-docs-view-controller
- luz-docs-view-controller — *provides endpoint* — /documents/search
- view-controller search-based file dedup — *matched documents by* — name + byte size
- idempotent re-import — *skips files recorded in* — previous job's history
- view-controller search-based file dedup — *involved DTO* — SearchDocumentResponse
- view-controller search-based file dedup — *involved DTO* — SearchDocumentResult
- view-controller search-based file dedup — *involved DTO* — Files
- view-controller search-based file dedup — *involved DTO* — Reference
- view-controller search-based file dedup — *involved DTO* — QueryRequest
- view-controller search-based file dedup — *involved DTO* — Query
- view-controller search-based file dedup — *involved DTO* — Term
- view-controller search-based file dedup — *involved method* — Archive.isSameParentFolder
- view-controller search-based file dedup — *involved method* — JsonUtils.converObjectToJson
- view-controller search-based file dedup — *involved method* — LuzDocsViewControllerService.searchDocument
- view-controller search-based file dedup — *used skip mechanism* — JobProgressWriter.recordSkipped(File)
- JobProgressWriter.recordSkipped(File) — *is* — removed
- idempotent re-import — *uses skip mechanism* — JobProgressWriter.recordSkippedAll(List)
- JobProgressWriter.recordSkippedAll(List) — *is* — kept
- ch.klara.luz.docsimport — *implements* — luz_docs_import
- ch.klara.luz.docsimport — *uses technology* — Java 17
- ch.klara.luz.docsimport — *uses technology* — WildFly EJB
- ch.klara.luz.docsimport — *uses storage* — json-store
- ch.klara.luz.docsimport — *supports feature* — eArchive ZIP import
- luz_docs_import — *is related to decision* — Read-time non-persisted state repair is an anti-pattern; use a scheduled reconciler

%% ai-graph-end %%