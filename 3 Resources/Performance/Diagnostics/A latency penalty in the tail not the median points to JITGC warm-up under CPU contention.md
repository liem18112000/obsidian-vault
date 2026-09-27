---
ai_hash: 38be3958140c50f6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities: []
source: luz-docs-import run-1 cold-start investigation 2026-08-13
status: seedling
tags:
- performance
- jit
- gc
- latency
- benchmarking
- diagnostics
title: A latency penalty in the tail not the median points to JIT/GC warm-up under
  CPU contention
type: technique
---

# A latency penalty in the tail not the median points to JIT/GC warm-up under CPU contention

When an operation is slow only on its first/cold execution and speeds up on repeats, look at **where in the latency distribution** the penalty lives. If the **median is roughly flat cold-vs-warm but the tail (p90/p99) is 2–4× fatter when cold and shrinks run-over-run**, the cause is **warm-up — HotSpot JIT compilation and/or GC competing for CPU** — not network or connection-pool overhead.

**Why the shape tells you this:** a network / connection cost shifts the *whole* distribution (median included) roughly uniformly. JIT/GC-under-contention instead adds a **decaying tail**: a few requests stall while compiler/GC threads steal cores from the workers, and the effect fades as methods finish compiling. It's especially pronounced on a **CPU-limited container** (few cores) running a concurrent worker pool, where JIT-compiler + GC threads directly contend with the workers.

**Confirm it:** watch per-op latency *within* the first run — if it roughly halves from the first decile to the last, that's in-run warm-up, not a fixed per-op cost.

Related: [[Truncating DB collections between benchmark runs resets data but not service warmth]], [[Small per-run batches warm the JIT gradually across runs; large batches warm within one run]].

## Related

- [[Truncating DB collections between benchmark runs resets data but not service warmth]]
- [[Small per-run batches warm the JIT gradually across runs; large batches warm within one run]]

%% ai-graph-start %%

**Related notes:**
- [[Small per-run batches warm the JIT gradually across runs; large batches warm within one run]]
- [[Truncating DB collections between benchmark runs resets data but not service warmth]]
- [[Trust median and p90 over many runs, not the mean of a few, for latency on a noisy shared env]]
- [[luz-docs-import cold first-import slowness is JIT plus downstream re-warm on a CPU-limited pod]]
- [[Concurrency-bound single-primary Mongo reads indexes stop helping; recognize by bimodal latency]]

%% ai-graph-end %%