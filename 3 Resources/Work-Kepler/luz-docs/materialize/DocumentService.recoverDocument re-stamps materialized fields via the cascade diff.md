---
ai_hash: 14d9e82b6836822b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-15
entities: []
source: session 2026-06-15
status: seedling
tags:
- luz-docs
- materialize
- recover
title: DocumentService.recoverDocument re-stamps materialized fields via the cascade
  diff
type: concept
---

# DocumentService.recoverDocument re-stamps materialized fields via the cascade diff

On recovering a soft-deleted document, `DocumentService.recoverDocument` re-stamps the materialized folder fields (`_isPublic`, `_effectiveSecurityClassCodes`, `_folderNames`) when the materialize cascade should run:

```java
if (materializeFacade.shouldCascadeDocument(tenantId, metadataBeforeRecovery, currentMetadata)) {
    currentMetadata = JsonObjectUtil.mergeTechnicalFields(
        currentMetadata, materializeFacade.onDocumentChange(token, tenantId, currentMetadata));
}
```

`metadataBeforeRecovery` is the deleted doc as-is; `currentMetadata` is after the folderIds (from the restore body) and deletionStatus changes are applied. `MaterializeCascadeService.shouldCascadeDocument` fires when folderIds differ (compared ORDER-SENSITIVELY, because `_folderNames`/codes are positionally aligned) OR securityClassCode differs (compared as an ORDER-INSENSITIVE set).

Testing guidance: assert the OBSERVABLE outcome — after recovery the materialized fields reflect the document's final folder membership + its own doc-level codes — rather than whether the cascade internally fired (you cannot observe that, and soft-delete may itself have changed folderIds).

Related: [[JSON-driven Scenario Outline pattern for luz_docs materialize integration tests]], [[document_service.recover_document drops an empty folderIds list, so it cannot clear folders]]

## Related

- [[JSON-driven Scenario Outline pattern for luz_docs materialize integration tests]]
- [[document_service.recover_document drops an empty folderIds list, so it cannot clear folders]]

%% ai-graph-start %%

**Related notes:**
- [[FolderService.recoverFolder is not materialize-aware]]
- [[document_service.recover_document drops an empty folderIds list, so it cannot clear folders]]
- [[Folder recovery reuses the parent-change materialize cascade]]
- [[luz_docs has two materialize cascade delivery mechanisms]]
- [[Folder recovery must recompute inherited security after deletion statuses are cleared]]

%% ai-graph-end %%