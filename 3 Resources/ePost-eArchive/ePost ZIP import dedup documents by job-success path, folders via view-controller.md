---
ai_hash: 5b92ef38dd89f23f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities: []
source: LUZ-158230 interrogation, session 2026-09-14
status: seedling
tags:
- luz
- ePost
- eArchive
- dedup
- idempotency
- LUZ-158230
title: 'ePost ZIP import dedup: documents by job-success path, folders via view-controller'
type: lesson
---

# ePost ZIP import dedup: documents by job-success path, folders via view-controller

Re-importing the same ZIP is idempotent via **two different dedup mechanisms**:

- **Documents**: deduped against **luz_docs_import job-success-file records**, keyed on the documents **full path**. A document already recorded as successfully imported (same full path) is deduped.
- **Folders**: luz_docs_import **calls luz_docs_view_controller** to search for an existing folder and **merge into it** rather than creating a duplicate.

So a repeat delivery lands documents/folders idempotently, but through two distinct code paths — worth separate test cases. Established for LUZ-158230.

## Related
[[ePost eArchive document write-chain import to view-controller to luz_docs to jsonstore|ePost eArchive document write-chain: import to view-controller to luz_docs to jsonstore]]
[[ePost eArchive ZIP import transfer.zip shape and luz_docs_import|ePost eArchive ZIP import: transfer.zip shape and luz_docs_import]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_import dedup folders via view-controller API, documents via import job history]]
- [[luz_docs_import ZIP import is path-based idempotent per importZipName]]
- [[ePost eArchive document write-chain import to view-controller to luz_docs to jsonstore]]
- [[ePost eArchive ZIP import transfer.zip shape and luz_docs_import]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]

%% ai-graph-end %%