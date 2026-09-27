---
ai_hash: b2103ef098f8eba4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-04
entities:
- FolderService.recoverFolder
- MaterializeFacade
- JSON store
- POST /{tenantId}/folders/{folder-id}/recovery
- FolderService
- restoreToNewParentFolderIds
- updateFolderMetadata
- inherited security
- path
- recoverSingleFolder
- deletionStatus
- deletionTimestamp
- executeRecovery
- documentService.recoverDocument
- allowlisted/materialization-complete tenants
- recovered subtree
- _folderNames
- _effectiveSecurityClassCodes
- security-classification correctness risk
- Non-materialized tenants
- MaterializeFolderRecoveryService
- materializeCascade
- shouldUseMaterialized(tenantId)
- LUZ-155107
- luz_docs has two materialize cascade delivery mechanisms
- Folder recovery re-parenting must recompute inheritedSecurityClassCode like the
  PUT path
- Materialization
- parent-change full-recompute pipeline
source: LUZ-155107 investigation, session 2026-06-04
status: budding
tags:
- luz-docs
- materialize
- folder-recovery
- security
- bug
title: FolderService.recoverFolder is not materialize-aware
type: observation
---

# FolderService.recoverFolder is not materialize-aware

The folder recovery API (`POST /{tenantId}/folders/{folder-id}/recovery` → `FolderService.recoverFolder`) restores state by writing straight to the JSON store and never touches `MaterializeFacade` — even though FolderService injects it for the rename/parent-change paths.

The three write sites, none of which cascade:
- `restoreToNewParentFolderIds` — re-parents via raw `updateFolderMetadata` (changes inherited security + path!)
- `recoverSingleFolder` — flips `deletionStatus`/clears `deletionTimestamp`, raw write
- `executeRecovery` — recovers subtree documents via `documentService.recoverDocument` (no materialize stamping)

Consequence for allowlisted/materialization-complete tenants: the recovered subtree keeps stale `_folderNames` and stale `_effectiveSecurityClassCodes` — a security-classification correctness risk (docs surfaced under wrong security filter or wrongly hidden). Worst case is recovery-with-re-parenting. Non-materialized tenants are unaffected.

Fix (LUZ-155107, sprint 158): new async `MaterializeFolderRecoveryService` — rename-style event + marker + PARTIAL retry, but in its OWN marker collection (isolated from rename's `materializeCascade`), executing the parent-change full-recompute pipeline over root + ALL descendants; fires in finally even on partial recovery; gated by `shouldUseMaterialized(tenantId)`.

Related: [[luz_docs has two materialize cascade delivery mechanisms]], [[Folder recovery re-parenting must recompute inheritedSecurityClassCode like the PUT path]]

## Related

- [[luz_docs has two materialize cascade delivery mechanisms]]
- [[Folder recovery re-parenting must recompute inheritedSecurityClassCode like the PUT path]]

%% ai-graph-start %%

**Related notes:**
- [[Folder recovery re-parenting must recompute inheritedSecurityClassCode like the PUT path]]
- [[Folder recovery reuses the parent-change materialize cascade]]
- [[Folder recovery must recompute inherited security after deletion statuses are cleared]]
- [[luz_docs has two materialize cascade delivery mechanisms]]
- [[Folder recovery with re-parenting leaves inheritedSecurityClassCode stale]]

**Relations:**
- FolderService.recoverFolder — *lacks awareness of* — Materialization
- POST /{tenantId}/folders/{folder-id}/recovery — *invokes* — FolderService.recoverFolder
- FolderService.recoverFolder — *writes state to* — JSON store
- FolderService.recoverFolder — *does not touch* — MaterializeFacade
- FolderService — *injects* — MaterializeFacade
- MaterializeFacade — *supports* — rename/parent-change paths
- FolderService.recoverFolder — *implements via* — restoreToNewParentFolderIds
- FolderService.recoverFolder — *implements via* — recoverSingleFolder
- FolderService.recoverFolder — *implements via* — executeRecovery
- restoreToNewParentFolderIds — *uses* — updateFolderMetadata
- updateFolderMetadata — *changes* — inherited security
- updateFolderMetadata — *changes* — path
- recoverSingleFolder — *flips* — deletionStatus
- recoverSingleFolder — *clears* — deletionTimestamp
- executeRecovery — *uses* — documentService.recoverDocument
- recovered subtree — *retains stale* — _folderNames
- recovered subtree — *retains stale* — _effectiveSecurityClassCodes
- stale _effectiveSecurityClassCodes — *leads to* — security-classification correctness risk
- Non-materialized tenants — *are unaffected by* — FolderService.recoverFolder
- MaterializeFolderRecoveryService — *fixes* — LUZ-155107
- MaterializeFolderRecoveryService — *is isolated from* — materializeCascade
- MaterializeFolderRecoveryService — *performs* — parent-change full-recompute pipeline
- MaterializeFolderRecoveryService — *is gated by* — shouldUseMaterialized(tenantId)
- FolderService.recoverFolder — *is related to* — luz_docs has two materialize cascade delivery mechanisms
- FolderService.recoverFolder — *is related to* — Folder recovery re-parenting must recompute inheritedSecurityClassCode like the PUT path

%% ai-graph-end %%