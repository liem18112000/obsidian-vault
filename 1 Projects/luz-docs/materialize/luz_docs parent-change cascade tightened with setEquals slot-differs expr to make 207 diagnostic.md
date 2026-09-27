---
ai_hash: aea25d96beb10f9b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-08
entities:
- luz_docs parent-change cascade
- setEquals slot-differs expression
- HTTP 207 diagnostic
- 'finding #25'
- eArchive materialise code review
- sprint-158
- updateManyByFilter
- loose filter
- _folderIds $in affectedFolderIds filter
- HTTP 207
- partial write
- benign HTTP 207
- MaterializeCascadeException
- '@Retry'
- '@Fallback'
- snapshot
- MaterializeMultiStatusException
- folder collection
- repo unit test
- Materialize folder parentFolderIds change cascade
- LUZ-154159
- Tight updateMany filter
- reliable partial-write signal
- Materialize code review report
- sprint-156 findings index
- Option A
- $expr operator
- $anyElementTrue operator
- $map operator
- $indexOfArray operator
- _folderSecurityClassCodes
- set-inequality check
- $in prefilter
- computed-unions map keys
- folder positions
- affected folders
- existing folders
- folders with computable union
source: 'luz_docs #25 sprint-158 fix-existing-bugs'
status: seedling
tags:
- luz-docs
- materialize
- earchive
- mongodb
- cascade
- LUZ-154159
title: luz_docs parent-change cascade tightened with setEquals slot-differs expr to
  make 207 diagnostic
type: lesson
---

# luz_docs parent-change cascade tightened with setEquals slot-differs expr to make 207 diagnostic

Fix for finding #25 (eArchive materialise code review, sprint-158). The parent-change cascade re-stamps doc sentinels via a deterministic `updateManyByFilter` whose filter was **loose**: `{_folderIds:{$in: affectedFolderIds}}`. That matched every doc touching an affected folder, including ones already carrying the correct codes, so HTTP 207 was expected on success yet identical to a real partial write — the code's blanket 'benign 207' swallow was unsound. (Background: [[Tight updateMany filter makes HTTP 207 a reliable partial-write signal]].)

**Option A applied:** tighten the filter to mirror the existing tight folder-rename filter. Keep `_folderIds $in` (index-friendly prefilter) **and** add a `$expr`:

- `$anyElementTrue` over a `$map` of the doc's folder positions `range(0, size(_folderIds))`;
- per position: `$indexOfArray(affectedIdsLiteral, _folderIds[i])` → if >= 0, the folder is affected, so test whether its existing `_folderSecurityClassCodes[i]` slot is **not** set-equal to that folder's freshly-computed (own ∪ inherited) union;
- set-inequality encoded as `{$eq:[{$setEquals:[slot, union]}, false]}`.

Result: only docs that genuinely need re-stamping match ⇒ matched==modified on success ⇒ any 207 = real partial write, wrapped as retryable `MaterializeCascadeException` (`@Retry` re-stamps still-stale docs, `@Fallback` rolls back via snapshot). Deleted the now-dead `MaterializeMultiStatusException` + its benign catch.

**Secondary effect:** `$in` is built from the computed-unions **map keys**, so it narrows to folders that actually exist / have a computable union. A folder absent from the folder collection has no union to stamp; the `$expr` still catches its docs via their other affected folders. Required updating a repo unit test that had mocked the folder fetch to return empty (the $in then collapses to size 0).

## Related

- [[Materialize folder parentFolderIds change cascade (LUZ-154159)]]
- [[Tight updateMany filter makes HTTP 207 a reliable partial-write signal]]
- [[Materialize code review report - sprint-156 findings index]]

%% ai-graph-start %%

**Related notes:**
- [[Tight updateMany filter makes HTTP 207 a reliable partial-write signal]]
- [[Deterministic Mongo pipeline updates return matched-not-modified; treat jsonstore SC_MULTI_STATUS as benign]]
- [[Materialize folder parentFolderIds change cascade (LUZ-154159)]]
- [[luz_docs parent-change cascade pipeline rebuilds _folderSecurityClassCodes positionally then re-derives the sentinels]]
- [[luz_docs parent-change cascade recovers forward, not via snapshot rollback]]

**Relations:**
- luz_docs parent-change cascade — *is tightened by* — setEquals slot-differs expression
- luz_docs parent-change cascade — *produces* — HTTP 207 diagnostic
- finding #25 — *is fixed by* — luz_docs parent-change cascade
- finding #25 — *originated from* — eArchive materialise code review
- eArchive materialise code review — *occurred in* — sprint-158
- luz_docs parent-change cascade — *uses* — updateManyByFilter
- updateManyByFilter — *previously used* — loose filter
- loose filter — *is defined as* — _folderIds $in affectedFolderIds filter
- loose filter — *caused* — benign HTTP 207
- benign HTTP 207 — *is indistinguishable from* — partial write
- Option A — *tightens* — loose filter
- Tight updateMany filter — *enables* — reliable partial-write signal
- reliable partial-write signal — *is* — HTTP 207
- Option A — *retains* — $in prefilter
- Option A — *adds* — $expr operator
- $expr operator — *incorporates* — $anyElementTrue operator
- $anyElementTrue operator — *operates over* — $map operator
- $map operator — *processes* — folder positions
- folder positions — *are checked by* — $indexOfArray operator
- $indexOfArray operator — *determines* — affected folders
- affected folders — *require checking* — _folderSecurityClassCodes
- _folderSecurityClassCodes — *is checked for* — set-inequality check
- set-inequality check — *is encoded as* — {$eq:[{$setEquals:[slot, union]}, false]}
- HTTP 207 — *signals* — partial write
- partial write — *is wrapped as* — MaterializeCascadeException
- MaterializeCascadeException — *is retryable via* — @Retry
- MaterializeCascadeException — *is rolled back via* — @Fallback
- @Fallback — *uses* — snapshot
- MaterializeMultiStatusException — *was deleted* — null
- MaterializeMultiStatusException — *had* — benign catch
- $in prefilter — *is built from* — computed-unions map keys
- $in prefilter — *filters for* — existing folders
- $in prefilter — *filters for* — folders with computable union
- folder collection — *stores* — folders
- $expr operator — *catches docs via* — affected folders
- repo unit test — *was updated* — null
- repo unit test — *mocked* — folder fetch
- Materialize folder parentFolderIds change cascade — *is related to* — luz_docs parent-change cascade
- LUZ-154159 — *is ID for* — Materialize folder parentFolderIds change cascade
- Tight updateMany filter makes HTTP 207 a reliable partial-write signal — *is related to* — luz_docs parent-change cascade
- Materialize code review report — *includes* — sprint-156 findings index

%% ai-graph-end %%