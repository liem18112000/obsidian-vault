---
ai_hash: d68d14069ee17247
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
aliases:
- 'luz_docs benchmark: full count scan is a dead end for sub-second targets'
created: 2026-07-09
entities:
- luz_docs
- documents/count endpoint
- 128k documents
- sub-second performance
- 14.1s median
- 3.15s median
- 500ms target
- _shard-range fan-out
- MongoDB primary contention
- application thread starvation
- scanning
- epoch-keyed result cache
- index-covered COUNT_SCAN
- maintained cardinality structure
- exact counter
- Roaring bitmap
- HyperLogLog sketch
- estimated-count feature
- luz_docs countN badge
- luz_docs estimated-count POC
- CAS
- backfill gate
- HyperLogLog error
- small-range (linear-counting) regime
source: docs/perf-LUZ-154613-count-fanout-EXEC-SUMMARY.md + perf-LUZ-154613-count-scaling-findings-and-solution.md,
  session 2026-07-09
status: seedling
tags:
- luz-docs
- kepler
- mongodb
- performance
- benchmark
- count-optimization
title: luz_docs /documents/count is scan-bound and cannot reach sub-second at 128k
type: observation
---

# luz_docs /documents/count is scan-bound and cannot reach sub-second at 128k

Benchmark on a 128k-document luz_docs tenant, `POST /documents/count` (materialized read path): a single count (K=1) takes **14.1s median**; the existing `_shard`-range fan-out at **K=6 gives 3.15s median**. Both are far over a 500ms target.

The fan-out floor (~3s regardless of K) is **MongoDB primary contention**, not application thread starvation: all K sub-counts run concurrently against the *same* replica-set primary on a non-sharded cluster and serialize once its capacity saturates. Confirmed by widening the executor pool to 64 threads — it did not beat the K=6 floor. The speedup also decays with data size (4.5x at 128k, ~1x by 480k) because the scan is never removed, only spread across cores.

**Implication:** scanning the matching set — serial or parallel — is a dead end for sub-second counts. The only paths to the target stop scanning per request: an epoch-keyed result cache, an index-covered `COUNT_SCAN` (materialize the predicate into an indexed boolean sentinel so `count()` reads index keys with no document FETCH), or a maintained cardinality structure (exact counter, Roaring bitmap, or an approximate HyperLogLog sketch). This is the fact that justified investigating HLL for the estimated-count feature at all.

## Related

- [[luz_docs countN badge can use HyperLogLog with a fuzzy-zone fallback]]
- [[luz_docs estimated-count POC drops CAS and backfill gate]]
- [[HyperLogLog error in the small-range (linear-counting) regime]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs documentscount is ~130s on an 800k tenant — the 16-shard fan-out, not counting, is the bottleneck]]
- [[luz_docs countN badge can use HyperLogLog with a fuzzy-zone fallback]]
- [[Frozen JsonStore gateway makes _id-range count fan-out a dead end — pivot to bitmapHLL]]
- [[Production security count is already COUNT_SCAN (covered); benchmark query's FETCH is inherent (multikey+$or+$nin)]]
- [[eArchive count baseline latency on dev ~80s for 128k docs (fan-out off)]]

**Relations:**
- luz_docs — *has* — documents/count endpoint
- documents/count endpoint — *is* — scan-bound
- documents/count endpoint — *cannot achieve* — sub-second performance
- documents/count endpoint — *benchmarked with* — 128k documents
- documents/count endpoint — *takes* — 14.1s median
- documents/count endpoint — *takes* — 3.15s median
- 14.1s median — *exceeds* — 500ms target
- 3.15s median — *exceeds* — 500ms target
- _shard-range fan-out — *is limited by* — MongoDB primary contention
- MongoDB primary contention — *is not* — application thread starvation
- scanning — *is a dead end for* — sub-second performance
- epoch-keyed result cache — *is a solution for* — sub-second performance
- index-covered COUNT_SCAN — *is a solution for* — sub-second performance
- maintained cardinality structure — *is a solution for* — sub-second performance
- exact counter — *is a type of* — maintained cardinality structure
- Roaring bitmap — *is a type of* — maintained cardinality structure
- HyperLogLog sketch — *is a type of* — maintained cardinality structure
- HyperLogLog sketch — *justified investigation of* — estimated-count feature
- luz_docs countN badge — *can use* — HyperLogLog sketch
- luz_docs estimated-count POC — *investigates* — estimated-count feature
- luz_docs estimated-count POC — *drops* — CAS
- luz_docs estimated-count POC — *drops* — backfill gate
- HyperLogLog error — *occurs in* — small-range (linear-counting) regime

%% ai-graph-end %%