---
title: "ePost ZIP import dedup: documents by job-success path, folders via view-controller"
created: 2026-09-14
type: lesson
status: seedling
source: "LUZ-158230 interrogation, session 2026-09-14"
tags: [luz, ePost, eArchive, dedup, idempotency, LUZ-158230]
---

# ePost ZIP import dedup: documents by job-success path, folders via view-controller

Re-importing the same ZIP is idempotent via **two different dedup mechanisms**:

- **Documents**: deduped against **luz_docs_import job-success-file records**, keyed on the documents **full path**. A document already recorded as successfully imported (same full path) is deduped.
- **Folders**: luz_docs_import **calls luz_docs_view_controller** to search for an existing folder and **merge into it** rather than creating a duplicate.

So a repeat delivery lands documents/folders idempotently, but through two distinct code paths — worth separate test cases. Established for LUZ-158230.

## Related
[[ePost eArchive document write-chain: import to view-controller to luz_docs to jsonstore]]
[[ePost eArchive ZIP import: transfer.zip shape and luz_docs_import]]
