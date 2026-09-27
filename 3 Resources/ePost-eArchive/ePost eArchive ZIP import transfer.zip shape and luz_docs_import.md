---
ai_hash: 5ac97a934973246d
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
- luz-docs-import
- zip-import
- LUZ-158230
title: 'ePost eArchive ZIP import: transfer.zip shape and luz_docs_import'
type: concept
---

# ePost eArchive ZIP import: transfer.zip shape and luz_docs_import

An external sender (e.g. Post Health) delivers documents into a recipient's ePost **eArchive** by uploading a **transfer.zip**. The ZIP carries the document binaries, a folder hierarchy, and a per-document sidecar named `<document file name>.metadata.json` describing each file.

Rules that hold with no ambiguity:
- Folders inside the ZIP become the eArchive folder structure **1:1** (folders created as needed).
- Every document has a sibling `<name>.metadata.json`.
- OS artefacts are **dropped silently** (no warning/error): `.DS_Store`, `Thumbs.db`, `__MACOSX/`.
- Built in the **luz_docs_import** service.

Required sidecar fields include `senderTenantId` (UUID), `senderCompanyId` (int), `senderName`, `documentTitle`. See the Confluence spec **ePost — ZIP Import Test Fixture Matrix** (pageId 49665769474) for the fixture cases. Established for ticket LUZ-158230.

## Related
[[HEALTH document type carries verbatim SNOMED healthData]]
[[ePost eArchive document write-chain import to view-controller to luz_docs to jsonstore|ePost eArchive document write-chain: import to view-controller to luz_docs to jsonstore]]
[[luz_docs_import scope no sender auth, individual tenants, partial-import policy|luz_docs_import scope: no sender auth, individual tenants, partial-import policy]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[ePost eArchive document write-chain import to view-controller to luz_docs to jsonstore]]
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[luz_docs_import scope no sender auth, individual tenants, partial-import policy]]
- [[ePost ZIP import dedup documents by job-success path, folders via view-controller]]

%% ai-graph-end %%