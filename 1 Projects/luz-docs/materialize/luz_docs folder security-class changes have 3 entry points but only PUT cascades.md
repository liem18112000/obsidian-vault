---
ai_hash: bedd8397b47412f6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-07
entities:
- luz_docs
- folder security-class changes
- PUT
- PATCH
- updateSecurityClasses endpoint
- updateFolderMetadata
- cascadeFolderParentChangeIfNeeded
- cascadeFolderRenameIfNeeded
- updatePatchFolderMetadata
- _securityClassCode
- _inheritedSecurityClassCode
- _parentFolderIds
- document sentinels
- _isPublic
- _effectiveSecurityClassCodes
- _folderSecurityClassCodes
- materialized search/count/get grants or denies
- FolderDeletingService
- Folder DELETE
- Folder CREATE
- sprint-156
- Deterministic Mongo pipeline updates
- jsonstore SC_MULTI_STATUS
- FolderService
source: materialize code review 2026-06-07
status: seedling
tags:
- luz-docs
- materialize
- security
- cascade
- earchive
title: luz_docs folder security-class changes have 3 entry points but only PUT cascades
type: observation
---

# luz_docs folder security-class changes have 3 entry points but only PUT cascades

A folder's security classes can change through three luz_docs entry points, but as of sprint-156 only one triggers the materialize document cascade:

1. **PUT** `updateFolderMetadata` (FolderService ~335) — calls both `cascadeFolderParentChangeIfNeeded` and `cascadeFolderRenameIfNeeded`. Cascades.
2. **PATCH** `updatePatchFolderMetadata` (~521) — handles `/_securityClassCode`, `/_inheritedSecurityClassCode`, `/_parentFolderIds` ops but only fires the rename cascade. No parent-change cascade.
3. **updateSecurityClasses endpoint** (`/folders/{id}/security-classes`, ~679) — builds a `/_securityClassCode` patch and routes through path 2. No cascade at all.

So PATCH-based security changes leave document sentinels (`_isPublic`, `_effectiveSecurityClassCodes`, `_folderSecurityClassCodes`) stale → materialized search/count/get grants or denies on old security. When adding any new folder-mutation path, wire `cascadeFolderParentChangeIfNeeded` (it diffs pre/post code unions itself — cheap if nothing changed). Folder DELETE is safe (FolderDeletingService re-stamps docs); folder CREATE needs nothing.

## Related

- [[Deterministic Mongo pipeline updates return matched-not-modified; treat jsonstore SC_MULTI_STATUS as benign]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs bulk folder PATCH runs the materialize cascade once per entry]]
- [[luz_docs parent-change cascade pipeline rebuilds _folderSecurityClassCodes positionally then re-derives the sentinels]]
- [[Put vs Patch UpdatingSecurityClassFolderProcess prefix denotes input shape, not logic]]
- [[Folder recovery reuses the parent-change materialize cascade]]
- [[Folder recovery must recompute inherited security after deletion statuses are cleared]]

**Relations:**
- folder security-class changes — *has entry point* — PUT
- folder security-class changes — *has entry point* — PATCH
- folder security-class changes — *has entry point* — updateSecurityClasses endpoint
- PUT — *cascades* — true
- PUT — *calls* — updateFolderMetadata
- updateFolderMetadata — *calls* — cascadeFolderParentChangeIfNeeded
- updateFolderMetadata — *calls* — cascadeFolderRenameIfNeeded
- PATCH — *calls* — updatePatchFolderMetadata
- updatePatchFolderMetadata — *handles* — _securityClassCode
- updatePatchFolderMetadata — *handles* — _inheritedSecurityClassCode
- updatePatchFolderMetadata — *handles* — _parentFolderIds
- PATCH — *fires* — rename cascade
- PATCH — *does not fire* — parent-change cascade
- updateSecurityClasses endpoint — *builds* — _securityClassCode
- updateSecurityClasses endpoint — *routes through* — PATCH
- updateSecurityClasses endpoint — *cascades* — false
- PATCH-based security changes — *leave* — document sentinels stale
- document sentinels — *include* — _isPublic
- document sentinels — *include* — _effectiveSecurityClassCodes
- document sentinels — *include* — _folderSecurityClassCodes
- document sentinels stale — *results in* — materialized search/count/get grants or denies
- Folder DELETE — *is handled by* — FolderDeletingService
- FolderDeletingService — *re-stamps* — docs
- Folder CREATE — *needs* — nothing
- folder security-class changes — *context is* — luz_docs
- PUT — *is part of* — FolderService
- folder security-class changes — *state as of* — sprint-156
- Deterministic Mongo pipeline updates — *is related to* — folder security-class changes
- jsonstore SC_MULTI_STATUS — *is related to* — folder security-class changes

%% ai-graph-end %%