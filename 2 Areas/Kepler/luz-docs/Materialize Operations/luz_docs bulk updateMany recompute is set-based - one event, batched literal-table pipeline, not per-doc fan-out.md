---
ai_hash: 9cef16b1bf754684
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-05
entities:
- luz_docs
- updateMany
- recompute
- BulkDocumentChangeEvent
- DocBulkMaterializeObserver
- BULK_RECOMPUTE_BATCH
- folderIds
- loadFoldersById
- _folderNames
- literal-table pipeline
- ALL FOUR sentinels
- id→name table
- parent-change cascade
- 16 MB command cap
- $range
- $ifNull
- WriteError
- updateManyByFilter
- '207'
- Mongo
- '@Retry'
- luz_docs change tracking covers updateMany-deleteMany via projected before-after
  snapshots keyed by id
- luz_docs DocumentChangeObserver base owns the reload-recompute-restamp template
- MongoDB forbids $lookup inside update pipeline (WriteError 72)
- $lookup
- WriteError 72
- design
- session handoff report
- modify-many code
- set-based
- per-doc fan-out
- batched
- pre-matched ids
- projected read
- folder prefetch
- distinct folderIds
- docs without folderIds
- empty arrays
- identical doc
- already-correct sentinels
- find→updateMany window
- idempotent recompute
- user decision
- working tree
- raw $size
source: DocBulkMaterializeObserver implementation, session 2026-06-05
status: budding
tags:
- luz-docs
- materialize
- change-tracking
- bulk-ops
- performance
- design-decision
title: luz_docs bulk updateMany recompute is set-based - one event, batched literal-table
  pipeline, not per-doc fan-out
type: model
---

# luz_docs bulk updateMany recompute is set-based - one event, batched literal-table pipeline, not per-doc fan-out

A tracked `updateMany` touching materialize trigger fields must NOT fan out N per-doc recompute events (~3N round trips: reload + recompute + re-stamp each). luz_docs instead fires **one** `BulkDocumentChangeEvent` carrying the pre-matched ids (no per-doc diff — bulk reactions recompute from current truth, so a diff would be unused), and `DocBulkMaterializeObserver` recomputes **set-based** per batch of `BULK_RECOMPUTE_BATCH` (200) ids:

1. one projected read of the batch docs → distinct folderIds,
2. one folder prefetch (`loadFoldersById`),
3. one `updateMany {_id: {$in: batch}}` with a literal-table pipeline that rebuilds ALL FOUR sentinels — including `_folderNames` via an id→name table, which the parent-change cascade deliberately skips but a bulk `folderIds` change invalidates.

Constant round trips per batch regardless of N; batching bounds both the `$in` and the inlined tables far below the 16 MB command cap.

Correctness props: `$range` input is `$ifNull`-guarded so docs without `folderIds` degrade to empty arrays (the by-id filter, unlike `{folderIds: {$in: ...}}`, does not guarantee the field exists — raw `$size` would WriteError); a 207 from `updateManyByFilter` (matched != modified) is swallowed as benign — Mongo does not count an update that produces an identical doc as modified, so already-correct sentinels are expected, not a partial failure; the find→updateMany window is the same accepted race as single-op tracking; idempotent recompute + @Retry converge.

## Related

- [[luz_docs change tracking covers updateMany-deleteMany via projected before-after snapshots keyed by id]]
- [[luz_docs DocumentChangeObserver base owns the reload-recompute-restamp template]]
- [[MongoDB forbids $lookup inside update pipeline (WriteError 72)]]

> [!warning] Status 2026-06-05: never merged — removed from the working tree the same day (user decision, "remove modify-many code for now"). The design lives in this note + the session handoff report only.

%% ai-graph-start %%

**Related notes:**
- [[luz_docs change tracking covers updateMany-deleteMany via projected before-after snapshots keyed by id]]
- [[luz-docs updateManyByFilter requires every targeted document to actually change]]
- [[Materialize bulk PATCH fans out into N serial per-doc PATCH calls]]
- [[luz_docs parent-change cascade pipeline rebuilds _folderSecurityClassCodes positionally then re-derives the sentinels]]
- [[MongoDB forbids $lookup inside update pipeline (WriteError 72)]]

**Relations:**
- luz_docs — *handles* — updateMany
- updateMany — *triggers* — recompute
- luz_docs — *uses* — set-based
- luz_docs — *avoids* — per-doc fan-out
- luz_docs — *fires* — BulkDocumentChangeEvent
- BulkDocumentChangeEvent — *carries* — pre-matched ids
- DocBulkMaterializeObserver — *recomputes* — set-based
- DocBulkMaterializeObserver — *recomputes per batch of* — BULK_RECOMPUTE_BATCH
- BULK_RECOMPUTE_BATCH — *is* — 200
- recompute — *involves* — projected read
- projected read — *yields* — distinct folderIds
- recompute — *involves* — folder prefetch
- folder prefetch — *uses* — loadFoldersById
- recompute — *involves* — literal-table pipeline
- literal-table pipeline — *rebuilds* — ALL FOUR sentinels
- ALL FOUR sentinels — *includes* — _folderNames
- _folderNames — *rebuilt via* — id→name table
- parent-change cascade — *skips* — _folderNames
- bulk folderIds change — *invalidates* — _folderNames
- batched — *bounds* — 16 MB command cap
- $range — *is guarded by* — $ifNull
- $ifNull — *handles* — docs without folderIds
- docs without folderIds — *degrade to* — empty arrays
- updateManyByFilter — *returns* — 207
- 207 — *is swallowed as* — benign
- Mongo — *does not count* — identical doc
- identical doc — *as* — modified
- already-correct sentinels — *are* — expected
- find→updateMany window — *is* — accepted race
- idempotent recompute — *converges* — 
- @Retry — *converges* — 
- luz_docs — *related to* — luz_docs change tracking covers updateMany-deleteMany via projected before-after snapshots keyed by id
- luz_docs — *related to* — luz_docs DocumentChangeObserver base owns the reload-recompute-restamp template
- MongoDB forbids $lookup inside update pipeline (WriteError 72) — *states* — MongoDB forbids $lookup
- $lookup — *causes* — WriteError 72
- design — *documented in* — this note
- design — *documented in* — session handoff report
- design — *status* — never merged
- design — *removed from* — working tree
- user decision — *caused removal of* — modify-many code
- raw $size — *causes* — WriteError
- raw $size — *without* — $ifNull

%% ai-graph-end %%