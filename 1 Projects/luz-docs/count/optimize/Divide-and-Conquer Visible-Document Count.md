---
ai_hash: 4079ce2ad7d19aa4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
aliases:
- Parallelize visible-doc count by fan-out over _id ranges (luz-docs)
created: 2026-06-16
entities:
- Divide-and-Conquer Count
- Concurrent Sub-Counts
- _id ranges
- Partial Sums
- _isPublic
- _effectiveSecurityClassCodes
- user.codes
- multikey index
- engine
- Amplification Factor
- request
- thread
- core
- _id
- ObjectId
- SPACE Constant
- ManagedExecutorService
- skew
- evenly-spaced cuts
- quantile boundaries
- Jira Ticket LUZ-154613
- Parallelization Package
- Mongo
- JsonStore gateway
- jsonStore.countByFilter
- match queries
- MaterializeRepository.countTotalDocumentByQuery
- MaterializeCountFanout.count
- Count Primitive Function
- MaterializeFacade
- Count Fanout Partitions Config
- Index shape
- creation timestamp
- uniform cuts
- uneven buckets
- CPU-bound
- I/O
- Roaring
- HyperLogLog
- bitmap union
- analytics secondary
- Technical Points for Beginners Note
- Count Scaling Path Note
- Frozen JsonStore Gateway Note
- _id-range count fan-out
- bitmapHLL
- Partitioning Strategy Note
- _countShard
- Partitioning
- Materialized Visible Document Total
- Visible Document Count Logic
- work
- index entry
- divide-conquer-count.excalidraw
- divide-conquer-count.png
source: LUZ-154613 (design + implementation), sessions 2026-05-30 / 2026-06-16
status: seedling
tags:
- luz-docs
- materialize
- mongodb
- performance
- count
- fanout
- objectid
title: Divide-and-Conquer Visible-Document Count
type: howto
---

# Divide-and-Conquer Visible-Document Count

> Split one slow count into **K concurrent sub-counts over disjoint `_id` ranges**, then sum the partials. Exact for any id distribution, because the ranges tile the whole space with no gap and no overlap.

Diagrams: [[divide-conquer-count.excalidraw|Overall]] · [[divide-conquer-count.png|Raw picture]] (step frames `s1-problem` … `s8-skew`).

## Why

The materialised visible-document total is `count(_isPublic == true OR _effectiveSecurityClassCodes ∩ user.codes)`. On a multikey index the engine pays for every `(document × matching-code)` index entry and then de-duplicates by record id, so work scales with that **amplification factor**, not with the answer. One request = one thread = one core → p99 tail for heavy users (many codes, many docs per code).

## Mechanism

- `_id` is a 24-hex ObjectId ≈ a number in `[0, 2^96)`. Place `K-1` evenly spaced cuts: `bound_i = SPACE * i / K` where `SPACE = BigInteger.ONE.shiftLeft(96)`, formatted as 24-char zero-padded lowercase hex.
- Sub-query `i` = base filter AND `{_id: {$gte: B(i-1), $lt: Bi}}`. First range open below (no `$gte`), last open above (no `$lt`).
- Run the K queries concurrently on a `ManagedExecutorService` (one per core), sum the partials.
- **Fail loud:** if any sub-count throws, the whole count fails — never return a partial sum. An under-count on a security-visibility total is an access-leak-shaped bug.
- If skew becomes a problem, swap the evenly-spaced cuts for quantile boundaries sampled from real data — that is the single point of change (correctness is unaffected either way).

## Implementation (LUZ-154613, package `ch.klara.luz.docs.materialize.parallelize`)

- **No Mongo change allowed** (Mongo sits behind the JsonStore gateway), so fan-out issues only ordinary match queries `{$and:[<base>, {_id:{$gte:'B(i-1)', $lt:'Bi'}}]}` through the existing `jsonStore.countByFilter`. Bounds are plain 24-char hex strings — the gateway already coerces `_id` string→ObjectId for eq/`$in`, so no `$oid` / extended-JSON wrapper is needed.
- **Wiring is one line:** `MaterializeRepository.countTotalDocumentByQuery` delegates to `MaterializeCountFanout.count(query, q -> jsonStore.countByFilter(...))`; the single count stays the per-range primitive, passed as a `ToIntFunction<JsonObject>`. `MaterializeFacade` above it is unchanged.
- **Config:** `luz.docs.materialize.count-fanout-partitions`, injected `@ConfigProperty(defaultValue="1") int`. `≤1` = fan-out OFF (safe default); `K` = K concurrent sub-counts.

## Limits

- **Index shape is mandatory:** `{_effectiveSecurityClassCodes:1, _id:1}` and `{_isPublic:1, _id:1}` — `_id` as the SECOND key so each range is an index seek *inside* the security-matched interval. Without it the range clause degrades to a post-filter: K× the work for zero gain.
- **Load skew:** ObjectIds carry a creation timestamp, so ids cluster by time and uniform cuts give uneven buckets. Correctness is unaffected; only the speedup caps.
- **Only helps when CPU-bound** with spare cores and a resident index. If the bottleneck is I/O (cold index pages), K sub-counts buy contention, not speed.
- **Amplification remains** — fan-out parallelises it, never removes it; ceiling = core count. Removing it needs a bitmap union (Roaring exact / HyperLogLog approximate).
- No read-preference knob exists on the count path, so heavy sub-counts cannot be routed to an analytics secondary.

## Related

- [[Divide-and-Conquer Count - Technical Points for Beginners]]
- [[Count-scaling path fan-out first, Roaring next, HyperLogLog for approximate]]
- [[Frozen JsonStore gateway makes _id-range count fan-out a dead end — pivot to bitmapHLL]]
- [[Partition the materialized count on a uniform _countShard int, not _id]]

%% ai-graph-start %%

**Related notes:**
- [[Partition the materialized count on a uniform _countShard int, not _id]]
- [[Production security count is already COUNT_SCAN (covered); benchmark query's FETCH is inherent (multikey+$or+$nin)]]
- [[Frozen JsonStore gateway makes _id-range count fan-out a dead end — pivot to bitmapHLL]]
- [[Levers to optimise the visible-document count beyond _shard fan-out]]
- [[Count fan-out _shard index must put _shard LAST in the compound key (ESR)]]

**Relations:**
- Divide-and-Conquer Count — *splits into* — Concurrent Sub-Counts
- Concurrent Sub-Counts — *operate over* — _id ranges
- Divide-and-Conquer Count — *sums* — Partial Sums
- Materialized Visible Document Total — *is calculated by* — Visible Document Count Logic
- Visible Document Count Logic — *involves* — _isPublic
- Visible Document Count Logic — *involves* — _effectiveSecurityClassCodes
- Visible Document Count Logic — *involves* — user.codes
- engine — *uses* — multikey index
- multikey index — *has* — index entry
- work — *scales with* — Amplification Factor
- request — *uses* — thread
- thread — *uses* — core
- _id — *is a type of* — ObjectId
- ObjectId — *is* — 24-hex
- _id ranges — *are calculated using* — SPACE Constant
- SPACE Constant — *is defined as* — BigInteger.ONE.shiftLeft(96)
- Concurrent Sub-Counts — *run on* — ManagedExecutorService
- skew — *is a problem for* — evenly-spaced cuts
- quantile boundaries — *is an alternative to* — evenly-spaced cuts
- Divide-and-Conquer Count — *is described by* — Jira Ticket LUZ-154613
- Divide-and-Conquer Count — *is implemented in* — Parallelization Package
- Mongo — *is behind* — JsonStore gateway
- Divide-and-Conquer Count — *uses* — jsonStore.countByFilter
- jsonStore.countByFilter — *processes* — match queries
- match queries — *include* — _id
- MaterializeRepository.countTotalDocumentByQuery — *delegates to* — MaterializeCountFanout.count
- MaterializeCountFanout.count — *uses* — Count Primitive Function
- Count Primitive Function — *is* — jsonStore.countByFilter
- MaterializeFacade — *is above* — MaterializeRepository.countTotalDocumentByQuery
- Count Fanout Partitions Config — *configures* — Concurrent Sub-Counts
- Index shape — *requires* — _effectiveSecurityClassCodes
- Index shape — *requires* — _isPublic
- Index shape — *requires* — _id
- _id — *is the second key in* — Index shape
- ObjectId — *has* — creation timestamp
- uniform cuts — *cause* — uneven buckets
- skew — *is caused by* — uniform cuts
- Divide-and-Conquer Count — *is effective when* — CPU-bound
- I/O — *is a bottleneck for* — Concurrent Sub-Counts
- Amplification Factor — *persists with* — Divide-and-Conquer Count
- bitmap union — *removes* — Amplification Factor
- Roaring — *is a type of* — bitmap union
- HyperLogLog — *is a type of* — bitmap union
- analytics secondary — *cannot be used for* — Concurrent Sub-Counts
- Divide-and-Conquer Count — *is related to* — Technical Points for Beginners Note
- Divide-and-Conquer Count — *is related to* — Count Scaling Path Note
- Frozen JsonStore Gateway — *makes* — _id-range count fan-out
- _id-range count fan-out — *a* — dead end
- bitmapHLL — *is a pivot from* — _id-range count fan-out
- Divide-and-Conquer Count — *is related to* — Frozen JsonStore Gateway Note
- Partitioning Strategy Note — *proposes* — _countShard
- _countShard — *as alternative to* — _id
- _countShard — *for* — Partitioning
- divide-conquer-count.excalidraw — *illustrates* — Divide-and-Conquer Count
- divide-conquer-count.png — *illustrates* — Divide-and-Conquer Count

%% ai-graph-end %%