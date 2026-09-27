---
ai_hash: a21ba25c0524dd43
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-16
entities:
- JsonStore gateway
- _id-range count fan-out
- bitmap/HLL
- MongoDB
- _id
- ObjectId
- $expr + $toObjectId
- index
- luz.docs.materialize.count-fanout-partitions
- Roaring bitmap
- HyperLogLog
- amplification-removing approach
- Quantile boundaries
- Divide-and-Conquer Visible-Document Count
- performance
- amplification
- client
- MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index
  (full scan)
- $in
- $gte
- $lt
- _id RANGES
- scan work
- count-side
source: LUZ-154613 session 2026-06-16
status: seedling
tags:
- luz-docs
- materialize
- mongodb
- performance
- decision
- objectid
title: Frozen JsonStore gateway makes _id-range count fan-out a dead end — pivot to
  bitmap/HLL
type: argument
---

# Frozen JsonStore gateway makes _id-range count fan-out a dead end — pivot to bitmap/HLL

Under the hard constraint that luz_jsonstore (the MongoDB gateway) cannot be changed, the divide-and-conquer visible-document count that partitions on _id RANGES is a dead end for performance:

- The gateway coerces a hex _id string to ObjectId only for equality and $in (both index-using), NOT inside $gte/$lt. So a native, index-seeking _id range cannot be expressed from the client.
- The only correct client-side _id range is $expr + $toObjectId, which full-scans (no index) — proven: each of 4 sub-counts took ~9s including the empty ones; K=4 wall 13s vs single indexed count 3.5s. Slower.
- No other partition key works: no uniform numeric field exists on the docs, and adding a compound index is also a forbidden Mongo change.

Therefore K sub-counts can be correct OR fast, never both, with a frozen gateway. Quantile boundaries (DESIGN §8) only balance scan work; they don't make it indexed, so they don't help either.

Decision: keep fan-out OFF (luz.docs.materialize.count-fanout-partitions=1, the safe default) and pursue the amplification-removing approach instead — Roaring bitmap (exact) / HyperLogLog (approx) union counts, which are count-side and need no gateway change (see luz_docs/docs/bitmap-count-investigation.md). Fan-out parallelises amplified work but never removes the amplification (DESIGN §8), so it was the wrong lever for the heavy-user p99 tail.

## Related

- [[MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan)]]
- [[Divide-and-Conquer Visible-Document Count]]

%% ai-graph-start %%

**Related notes:**
- [[MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan)]]
- [[Partition the materialized count on a uniform _countShard int, not _id]]
- [[Divide-and-Conquer Visible-Document Count]]
- [[Mongo _id range with hex-string bounds matches nothing unless gateway coerces to ObjectId]]
- [[Levers to optimise the visible-document count beyond _shard fan-out]]

**Relations:**
- JsonStore gateway — *is a* — MongoDB gateway
- JsonStore gateway — *has constraint* — cannot be changed
- JsonStore gateway — *renders* — _id-range count fan-out
- _id-range count fan-out — *partitions on* — _id RANGES
- _id-range count fan-out — *negatively impacts* — performance
- JsonStore gateway — *coerces* — _id
- _id — *to* — ObjectId
- JsonStore gateway — *coerces for operator* — $in
- JsonStore gateway — *does not coerce* — _id
- JsonStore gateway — *does not coerce for operator* — $gte
- JsonStore gateway — *does not coerce for operator* — $lt
- _id range — *cannot be* — index-seeking
- _id range — *from* — client
- $expr + $toObjectId — *enables* — client-side _id range
- $expr + $toObjectId — *causes* — full-scan
- $expr + $toObjectId — *does not use* — index
- $expr + $toObjectId — *is* — correct
- _id-range count fan-out — *is not* — both correct and fast
- _id-range count fan-out — *due to* — JsonStore gateway constraint
- Quantile boundaries — *balances* — scan work
- Quantile boundaries — *does not enable* — index
- luz.docs.materialize.count-fanout-partitions — *is set to* — 1
- 1 — *is the* — safe default
- Decision — *is to pursue* — amplification-removing approach
- amplification-removing approach — *includes* — Roaring bitmap
- amplification-removing approach — *includes* — HyperLogLog
- Roaring bitmap — *provides* — exact counts
- HyperLogLog — *provides* — approximate counts
- Roaring bitmap — *is* — count-side
- HyperLogLog — *is* — count-side
- Roaring bitmap — *requires no* — JsonStore gateway change
- HyperLogLog — *requires no* — JsonStore gateway change
- _id-range count fan-out — *parallelises* — amplified work
- _id-range count fan-out — *does not remove* — amplification
- _id-range count fan-out — *is related to* — Divide-and-Conquer Visible-Document Count
- bitmap/HLL — *is a* — pivot
- bitmap/HLL — *is an* — amplification-removing approach
- MongoDB — *uses* — index
- MongoDB — *uses* — compound index
- MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan) — *describes* — $expr + $toObjectId
- MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan) — *is related to* — _id-range count fan-out
- Divide-and-Conquer Visible-Document Count — *is related to* — _id-range count fan-out
- bitmap/HLL — *is a solution for* — _id-range count fan-out limitations

%% ai-graph-end %%