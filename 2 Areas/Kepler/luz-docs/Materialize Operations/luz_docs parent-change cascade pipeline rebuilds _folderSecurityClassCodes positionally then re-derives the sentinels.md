---
ai_hash: d3192c3f9c573eb2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-05
entities:
- luz_docs
- parent-change cascade pipeline
- _folderSecurityClassCodes
- sentinels
- MaterializeComputeBuilder
- buildFolderParentChangePipeline
- $addFields pipeline
- updateMany
- folderIds
- Stage 1
- $map
- $range
- $size
- $indexOfArray
- literal id table
- Java-prefetched code unions
- $arrayElemAt
- $ifNull
- _folderNames
- Stage 2
- _effectiveSecurityClassCodes
- securityClassCodes
- $setUnion
- $reduce
- _isPublic
- $anyElementTrue
- MongoDB forbids $lookup inside update pipeline (WriteError 72)
- luz_docs onFolderParentsChange risk profile
- luz_docs materialize cascade delivery mechanisms
source: MaterializeComputeBuilder.java, session 2026-06-05
status: budding
tags:
- luz-docs
- materialize
- mongodb
- aggregation
title: luz_docs parent-change cascade pipeline rebuilds _folderSecurityClassCodes
  positionally then re-derives the sentinels
type: model
---

# luz_docs parent-change cascade pipeline rebuilds _folderSecurityClassCodes positionally then re-derives the sentinels

The parent-change cascade update (built by `MaterializeComputeBuilder.buildFolderParentChangePipeline`) is a 2-stage `$addFields` pipeline run via `updateMany` on docs matching `{folderIds: {$in: [affected...]}}`:

**Stage 1 — positional rebuild of `_folderSecurityClassCodes`.** `$map` over `{$range: [0, {$size: "$folderIds"}]}` (index `i`); for each slot, `$indexOfArray` looks the folder id up in an inlined literal id table (the Java-prefetched own ∪ inherited code unions — see the literal-table technique). Hit (`j >= 0`) → take `{$arrayElemAt: [<literal unions>, "$$j"]}`; miss → keep the existing slot `{$ifNull: [{$arrayElemAt: ["$_folderSecurityClassCodes", "$$i"]}, []]}` — unaffected folders' codes are still valid because a parent-change only alters the affected subtree. `_folderNames` is deliberately untouched (parent-chain-independent).

**Stage 2 — re-derive the doc-level sentinels from stage 1's output:**
- `_effectiveSecurityClassCodes` = `$setUnion` of the doc's own `securityClassCodes` and a `$reduce`/`$setUnion` fold over `_folderSecurityClassCodes`.
- `_isPublic` = doc has no own codes **AND** (it sits in no folder **OR** `$anyElementTrue` over a `$map` testing each per-folder code set for emptiness — i.e. at least one containing folder is fully open).

Every array access is `$ifNull`-guarded to `[]`, so short/missing parallel arrays degrade to "treat as empty" rather than erroring.

## Related

- [[MongoDB forbids $lookup inside update pipeline (WriteError 72)]]
- [[luz_docs onFolderParentsChange risk profile - sync fan-out, page-read gap, paging races]]
- [[luz_docs has two materialize cascade delivery mechanisms]]

%% ai-graph-start %%

**Related notes:**
- [[Materialize folder parentFolderIds change cascade (LUZ-154159)]]
- [[luz_docs folder security-class changes have 3 entry points but only PUT cascades]]
- [[luz_docs bulk folder PATCH runs the materialize cascade once per entry]]
- [[luz_docs has two materialize cascade delivery mechanisms]]
- [[_folderNames is parent-chain-independent — depends only on each folder's own name]]

**Relations:**
- luz_docs — *uses* — parent-change cascade pipeline
- parent-change cascade pipeline — *rebuilds* — _folderSecurityClassCodes
- parent-change cascade pipeline — *re-derives* — sentinels
- parent-change cascade pipeline — *is built by* — buildFolderParentChangePipeline
- buildFolderParentChangePipeline — *is a method of* — MaterializeComputeBuilder
- parent-change cascade pipeline — *is a type of* — $addFields pipeline
- parent-change cascade pipeline — *executes via* — updateMany
- updateMany — *targets documents with* — folderIds
- parent-change cascade pipeline — *includes* — Stage 1
- parent-change cascade pipeline — *includes* — Stage 2
- Stage 1 — *rebuilds* — _folderSecurityClassCodes
- Stage 1 — *uses* — $map
- $map — *iterates over* — $range
- $range — *uses* — $size
- Stage 1 — *uses* — $indexOfArray
- $indexOfArray — *queries* — literal id table
- literal id table — *contains* — Java-prefetched code unions
- Stage 1 — *uses* — $arrayElemAt
- Stage 1 — *uses* — $ifNull
- _folderNames — *is untouched by* — parent-change cascade pipeline
- Stage 2 — *re-derives* — sentinels
- Stage 2 — *processes output from* — Stage 1
- sentinels — *comprise* — _effectiveSecurityClassCodes
- sentinels — *comprise* — _isPublic
- _effectiveSecurityClassCodes — *is derived using* — $setUnion
- _effectiveSecurityClassCodes — *combines* — securityClassCodes
- _effectiveSecurityClassCodes — *combines* — _folderSecurityClassCodes
- _effectiveSecurityClassCodes — *combines via* — $reduce
- _isPublic — *is derived using* — $anyElementTrue
- _isPublic — *is derived using* — $map
- parent-change cascade pipeline — *is related to* — MongoDB forbids $lookup inside update pipeline (WriteError 72)
- parent-change cascade pipeline — *is related to* — luz_docs onFolderParentsChange risk profile
- luz_docs — *has* — luz_docs materialize cascade delivery mechanisms

%% ai-graph-end %%