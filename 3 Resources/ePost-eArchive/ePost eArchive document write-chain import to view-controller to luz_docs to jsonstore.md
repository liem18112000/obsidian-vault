---
ai_hash: 056cccf08603e5a4
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
- architecture
- write-chain
- luz-docs
- LUZ-158230
title: 'ePost eArchive document write-chain: import to view-controller to luz_docs
  to jsonstore'
type: model
---

# ePost eArchive document write-chain: import to view-controller to luz_docs to jsonstore

Creating a folder/document from a ZIP import fans out through a chain of services rather than luz_docs_import writing storage directly:

`luz_docs_import` -> `luz_docs_view_controller` (adds/derives extra params) -> `luz_docs` -> `luz_jsonstore` (creates folder/doc **metadata**) + **GCS** (stores the binary).

- **Antivirus** is in the chain (files are scanned).
- luz_docs_import does **not** own folder creation — it calls luz_docs_view_controller (ldvc) for that.
- The recipient-facing UI where imported eArchive folders/documents are viewed is **luz_epost_business_web**.

All backend repos live in the **axonivy-prod** Bitbucket workspace: `luz_docs_import`, `luz_docs_view_controller`, `luz_docs`, `luz_jsonstore`. This makes the import an inherently **multi-service integration** path for testing.

## Related
[[ePost eArchive ZIP import transfer.zip shape and luz_docs_import|ePost eArchive ZIP import: transfer.zip shape and luz_docs_import]]
[[ePost ZIP import dedup documents by job-success path, folders via view-controller|ePost ZIP import dedup: documents by job-success path, folders via view-controller]]

%% ai-graph-start %%

**Related notes:**
- [[ePost eArchive ZIP import transfer.zip shape and luz_docs_import]]
- [[ePost ZIP import dedup documents by job-success path, folders via view-controller]]
- [[luz-docs-import ZIP import call chain]]
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[LUZ-158230 test approach full-chain real-deps integration with fully-materialized done]]

%% ai-graph-end %%