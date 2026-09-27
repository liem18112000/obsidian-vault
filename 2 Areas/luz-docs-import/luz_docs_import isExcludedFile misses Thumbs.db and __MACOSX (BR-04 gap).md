---
ai_hash: 60aa3cd1320389ec
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-04
entities:
- luz_docs_import
- DocsImportAsyncService.isExcludedFile()
- DocsImportAsyncService.java
- BR-04
- Thumbs.db
- __MACOSX
- .DS_Store
- ._*
- eArchive
- createFolder
- Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection
source: LUZ-158230 investigation 2026-08-04
status: seedling
tags:
- luz-docs-import
- kepler
- gotcha
- zip-import
title: luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap)
type: observation
---

# luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap)

In luz_docs_import, `DocsImportAsyncService.isExcludedFile()` (DocsImportAsyncService.java:227) only excludes entries whose name `startsWith(".")`. Consequences for the spec BR-04 (ignore `__MACOSX/`, `.DS_Store`, `Thumbs.db`, dot-prefixed):

- `.DS_Store` and macOS `._*` resource forks → already skipped (dot-prefixed). OK.
- `Thumbs.db` → NOT dot-prefixed → currently IMPORTED as a document. Needs an explicit name check.
- `__MACOSX/` → every directory in the unzip reaches `createFolder` (the loop at :153-156 has no exclusion for dirs), so a macOS-authored zip currently CREATES a spurious `__MACOSX` folder in the eArchive. Needs the directory branch guarded.

So finishing BR-04 = extend isExcludedFile (Thumbs.db, under-__MACOSX) AND skip the `__MACOSX` directory in the folder-creation branch.

Related: [[Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection]]

## Related

- [[Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection]]

%% ai-graph-start %%

**Related notes:**
- [[Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection]]
- [[luz-docs-import bug rejected files not removed from unprocessedFiles]]
- [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]
- [[luz-docs-import AV scan covers only the metadata sidecar, never the document binary]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]

**Relations:**
- luz_docs_import — *has component* — DocsImportAsyncService.isExcludedFile()
- DocsImportAsyncService.isExcludedFile() — *defined in* — DocsImportAsyncService.java
- luz_docs_import — *has gap related to* — BR-04
- DocsImportAsyncService.isExcludedFile() — *misses exclusion for* — Thumbs.db
- DocsImportAsyncService.isExcludedFile() — *misses exclusion for* — __MACOSX
- BR-04 — *specifies ignoring* — __MACOSX
- BR-04 — *specifies ignoring* — .DS_Store
- BR-04 — *specifies ignoring* — Thumbs.db
- BR-04 — *specifies ignoring* — dot-prefixed
- .DS_Store — *is* — dot-prefixed
- ._* — *is* — dot-prefixed
- Thumbs.db — *is not* — dot-prefixed
- Thumbs.db — *is currently* — IMPORTED as a document
- __MACOSX — *creates spurious folder in* — eArchive
- createFolder — *is involved in creation of* — __MACOSX
- BR-04 — *requires extension of* — DocsImportAsyncService.isExcludedFile()
- DocsImportAsyncService.isExcludedFile() — *needs to exclude* — Thumbs.db
- DocsImportAsyncService.isExcludedFile() — *needs to exclude* — under-__MACOSX
- __MACOSX — *needs to be skipped in* — folder-creation branch
- luz_docs_import — *is related to* — Health ZIP import broken sidecar still imports; orphan sidecar is the only rejection

%% ai-graph-end %%