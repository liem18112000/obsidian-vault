---
title: "Folder recovery with re-parenting leaves inheritedSecurityClassCode stale"
created: 2026-06-04
updated: 2026-06-04
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49476173837/Folder+recovery+with+re-parenting+leaves+inheritedSecurityClassCode+stale
confluence_id: "49476173837"
confluence_path: "Team Kepler > Developer note"
tags: [confluence, security]
---

# Folder recovery with re-parenting leaves inheritedSecurityClassCode stale

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49476173837/Folder+recovery+with+re-parenting+leaves+inheritedSecurityClassCode+stale) · updated 2026-06-04*

## Overview

**Ticket:** LUZ-155107 (eArchive backend — cascade changes / folder recovery investigation)

**Scope:** `POST /{tenantId}/folders/{folder-id}/recovery` with a `restore.parentFolderIds` body

## Summary

When a soft-deleted folder is recovered **into a different parent** (`restore.parentFolderIds`), `FolderService.restoreToNewParentFolderIds` used to write the new `parentFolderIds` straight to the JSON store and stop. It never recomputed the folder's own `inheritedSecurityClassCode`, and never touched the subtree. The folder-side security metadata therefore no longer matched the folder's actual position in the tree.

This is a **pre-existing bug independent of the materialize feature** — it corrupts the source-of-truth folder documents, not just a denormalized view.

## How inherited security is supposed to flow

A folder's effective security = its own `securityClassCode` ∪ its `inheritedSecurityClassCode`, where the inherited part is derived from the parents:

- On **create** (`FolderService.buildFolderObject`): `getInheritedParentSecurityClass(tenantId, parentFolderIds)` collects `securityClassCode` + `inheritedSecurityClassCode` from every parent and stamps the union onto the new folder.

- On **PUT update** (`FolderService.updateFolderMetadata` → `processUpdatingFolderMetadataByPut`): after writing the metadata, `PutUpdatingSecurityClassFolderProcess.process()` runs when `parentFolderIds` changed. It:

  1.  recomputes `inheritedSecurityClassCode` from the **new** parents (`setNewInheritedSecurityClasses`),

  2.  patches the folder if the set changed (`buildPatchQuery` → REPLACE/ADD on `/inheritedSecurityClassCode`),

  3.  recursively re-runs itself on **every subfolder** (`doSubFolders` calls `.run()` directly, bypassing the changed-parents check) so the whole subtree re-derives its inheritance.

## Evidence

Exhibits from the code on this branch (pre-fix state for A/B/C; D shows the correct path that recovery skipped):

### **A — the raw write, no inheritance recompute**

(`FolderService.restoreToNewParentFolderIds`, pre-fix):

```
folderMetadata = JsonObjectUtil.updateFieldJsonArrayOfJsonObject(folderMetadata,
        FolderMetadataConstants.PARENT_FOLDER_IDS, parentFolders);
jsonStoreMongoService.updateFolderMetadata(tenantId, folderMetadata.getString(FolderMetadataConstants.ID), folderMetadata, token);
// <-- returns here; inheritedSecurityClassCode of folder + subtree never recomputed
```

### **B — what the PUT path does instead**

(`FolderService.processUpdatingFolderMetadataByPut`):

```
jsonStoreMongoService.updateFolderMetadata(tenantId, folderId, updatedMetadata, token);
PutUpdatingSecurityClassFolderProcess putUpdatingSecurityClassFolderProcess = new PutUpdatingSecurityClassFolderProcess(
        tenantId, folderId, currentMetadata, updatedMetadata, token, this, jsonStoreMongoService, folderUtil);
putUpdatingSecurityClassFolderProcess.process();   // <-- recovery never did this
```

### **C — access control reads the (stale) inherited codes**

(`FolderService.verifyTenantSecurityClasses`):

```
List<String> folderInheritedSecClasses = JsonObjectUtil.getSecurityClassesByField(
        FolderMetadataConstants.INHERITED_SECURITY_CLASSCODE, folder);
...
return CollectionUtil.isMatchingSecurityClasses(folderSecClasses, tenantSecurityClasses)
        || CollectionUtil.isMatchingSecurityClasses(folderInheritedSecClasses, tenantSecurityClasses);
```

### **D — the subtree recompute machinery that already existed** (`UpdatingSecurityClassFolderProcess`)

`setNewInheritedSecurityClasses()` derives the new set via `folderService.getInheritedParentSecurityClass(tenantId, updatedParentFolderIds)`; `doSubFolders()` re-runs the process on every subfolder via `.run()` (bypassing the changed-check), so one entry point re-derives the whole subtree.

### Flow comparison

![[image-20260604-015756.png]]

## The gap in the recovery path

`FolderService.restoreToNewParentFolderIds` (called from `recoverFolder` before the subtree walk) did only this:

```
folderMetadata = JsonObjectUtil.updateFieldJsonArrayOfJsonObject(folderMetadata,
        FolderMetadataConstants.PARENT_FOLDER_IDS, parentFolders);
jsonStoreMongoService.updateFolderMetadata(tenantId, folderMetadata.getString(ID), folderMetadata, token);
```

A raw write of `parentFolderIds` — no `PutUpdatingSecurityClassFolderProcess`, no subtree cascade.

### Consequences

Recover folder `F` (inherited `[HR]` from old parent `P_old`) into new parent `P_new` with security `[FIN]`:

|  |  |  |
|----|----|----|
| Field on `F` after recovery | Expected | Actual (before fix) |
| `parentFolderIds` | `[P_new]` | `[P_new]` ✓ |
| `inheritedSecurityClassCode` | `[FIN]` | `[HR]` ✗ (stale) |
| subtree of `F` | re-derived from `[FIN]` | stale `[HR]` everywhere ✗ |

Downstream effects:

1.  **Access control wrong at the source**: `getFolderById` / folder listing check `verifyTenantSecurityClasses` against the stale inherited codes — users with the old parent's classes keep access; users with the new parent's classes are denied.

2.  **Poisons every later recompute**: any doc-side materialize cascade (`MaterializeComputeBuilder` pipeline) reads the folder's *current* `securityClassCode + inheritedSecurityClassCode` via `$lookup`. With a stale folder-side field, even a "correct" cascade stamps stale effective security onto every document in the subtree. Fixing the doc-side cascade alone (the main LUZ-155107 change) is therefore **not sufficient** in the re-parent case.

3.  The staleness persists until some unrelated PUT/PATCH on the folder happens to trigger the security-class process.

### Why the bug stayed hidden

- Recovery without re-parenting doesn't change inheritance, so the common path looks fine.

- The stale field only shows up as wrong *authorization* behaviour, not as an error.

- `getSubFolders(token, tenantId, folderId, false)` includes soft-deleted folders, so nothing in the recovery flow ever had to re-read the security fields and notice the mismatch.

## Solution applied

Make the recovery re-parent path run the **same inheritance recompute as the PUT path**. In `FolderService.restoreToNewParentFolderIds`, after writing the new `parentFolderIds`:

```
new PutUpdatingSecurityClassFolderProcess(tenantId, updatedMetadata.getString(FolderMetadataConstants.ID),
        folderMetadata, updatedMetadata, token, this, jsonStoreMongoService, folderUtil).process();
```

- `process()` self-gates on `isChangingArrayValue(PARENT_FOLDER_IDS, current, updated)` — when the restore body carries the same parents the folder already has, nothing runs (no extra writes).

- When parents did change, the process recomputes `inheritedSecurityClassCode` from the new parents and cascades the re-derivation through every subfolder, exactly as a PUT re-parent would.

- Works on the still-soft-deleted subtree because `doSubFolders` → `getSubFolders(..., false)` includes deleted folders.

Ordering matters: this runs **before** `filterRecoveryFolder`/`executeRecovery` (unchanged call position in `recoverFolder`), so by the time the materialize recovery cascade fires (after recovery, async), the folder-side fields it `$lookup`s are already correct.

## Relation to the materialize recovery cascade (main LUZ-155107 change)

Two layers, both needed:

1.  **Folder-side (this doc)** — keep `inheritedSecurityClassCode` on the folder documents correct; source of truth for any recompute.

2.  **Doc-side** — `MaterializeFolderRecoveryService` re-stamps `_folderNames`, `_folderSecurityClassCodes`, `_effectiveSecurityClassCodes`, `_isPublic` on every document in the recovered subtree (async event + `materializeRecoveryCascade` marker + PARTIAL retry), gated by `MaterializeFacade.shouldUseMaterialized(tenantId)`.

Without (1), (2) faithfully materializes wrong data. Without (2), (1) is correct but materialized tenants still serve the stale snapshot.
