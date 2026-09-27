---
ai_hash: 5cf12dcfe6d9ed62
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-12
entities:
- Folder-deletion batching
- materialise cascade
- sprint-158 merge
- origin/master
- kepler/sprint-158/enhance-delete-folder-api
- FolderDeletingService.updateDocumentInRemoveFolder
- updateManyRemoveArrayValues
- materializeFacade.shouldCascadeDocument
- onDocumentChange
- technical fields
- e3ce6663b
- master per-doc flow
- branch batching
- luz_docs parent-change cascade recovers forward, not via snapshot rollback
source: merge e3ce6663b, session 2026-06-12
status: seedling
tags:
- luz-docs
- earchive
- merge
- materialize
- folder-deletion
title: Folder-deletion batching lost to materialise cascade in sprint-158 merge
type: observation
---

# Folder-deletion batching lost to materialise cascade in sprint-158 merge

Merging origin/master (eArchive materialise cascade) into kepler/sprint-158/enhance-delete-folder-api hit a semantic conflict in `FolderDeletingService.updateDocumentInRemoveFolder`: the branch had batched folder-unlink via one `updateManyRemoveArrayValues` call; master had per-document writes with `materializeFacade.shouldCascadeDocument` → `onDocumentChange` merging recomputed technical fields.

Resolution chosen (2026-06-12, merge e3ce6663b): **master per-doc flow wins; branch batching dropped in that method.** Reason: materialised technical fields are per-document values a shared updateMany pull cannot express. A hybrid (batch for non-cascade docs, per-doc only for cascade docs, cascade docs excluded from batch because updateManyRemoveArrayValues fails verification on matched-but-unmodified) was prototyped and discarded — viable template if the batching perf needs reviving.

## Related

- [[luz_docs parent-change cascade recovers forward, not via snapshot rollback]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs parent-change cascade recovers forward, not via snapshot rollback]]
- [[luz-docs delete-folder batching roadmap - remaining per-item paths]]
- [[LUZ-155107 shipped as two commits so the inheritedSecurityClassCode fix can cherry-pick to earchive-master]]
- [[luz_docs FolderDeletingServiceIT coverage gaps]]
- [[luz_docs folder delete shared document handling]]

**Relations:**
- Folder-deletion batching — *lost to* — materialise cascade
- Folder-deletion batching — *lost to* — sprint-158 merge
- sprint-158 merge — *merged* — origin/master
- sprint-158 merge — *merged* — kepler/sprint-158/enhance-delete-folder-api
- origin/master — *implemented* — materialise cascade
- kepler/sprint-158/enhance-delete-folder-api — *implemented* — branch batching
- FolderDeletingService.updateDocumentInRemoveFolder — *had semantic conflict in* — sprint-158 merge
- branch batching — *used* — updateManyRemoveArrayValues
- master per-doc flow — *used* — materializeFacade.shouldCascadeDocument
- materializeFacade.shouldCascadeDocument — *triggers* — onDocumentChange
- onDocumentChange — *merges* — technical fields
- e3ce6663b — *is resolution for* — sprint-158 merge
- e3ce6663b — *favored* — master per-doc flow
- e3ce6663b — *dropped* — branch batching
- technical fields — *are* — per-document values
- updateManyRemoveArrayValues — *cannot express* — per-document values
- sprint-158 merge — *related to* — luz_docs parent-change cascade recovers forward, not via snapshot rollback

%% ai-graph-end %%