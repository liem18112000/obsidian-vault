---
ai_hash: f659441faf27c5a1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities:
- luz-docs-import
- performance env
- dev env
- c6b1e23
- 45b05710…
- '2026-08-13'
- docs/tests/import-benchmark-2026-08-13_env_performance*
- 1k warm import
- 46 s
- 100 s
- 2.5k warm import
- 89 s
- 170 s
- 22 docs/s
- 28 docs/s
- 10 docs/s
- 15 docs/s
- cold-start
- 1.25×
- 2.4×
- antivirus-metadata timeouts
- '0'
- '3'
- downstream/AV headroom
- dedicated per-tenant Mongo shard
- 2 replicas
- 250-doc × 100 runs
- 100/100 DONE
- 250 imported
- 0 failures
- 33.6 s
- 50 s
- 935 s
- N=100
- 5k import
- high-variance
- shared env
- 196 s
- 1077 s
- other workloads
- uploads
- 5 s
- 26 s
- cluster load
- code
- reliability
- soak
- many small samples
- spikes
- Performance Mongo
- mongos-routed sharded cluster
- tenant's shard primary
- mongos
- median
- p90
- many runs
- latency
- noisy shared env
- mean
- few runs
- Small import batches
- overhead-bound
- per-item throughput
- large batches
- luz-docs-import cold first-import slowness
- JIT
- downstream re-warm
- CPU-limited pod
source: docs/tests/import-benchmark-2026-08-13_env_performance* 2026-08-13
status: seedling
tags:
- luz-docs-import
- performance
- benchmark
- import
title: luz-docs-import performance-env import benchmark findings
type: observation
---

# luz-docs-import performance-env import benchmark findings

Benchmark of luz-docs-import ZIP import on the **performance** env (same code/image `c6b1e23` as dev, tenant `45b05710…`, clean-before-each-run), 2026-08-13. Reports in `docs/tests/import-benchmark-2026-08-13_env_performance*`.

Findings:
- **Performance ≈ 2× faster than dev** on identical fixtures — 1k warm ~46 s (dev ~100 s), 2.5k warm ~89 s (dev ~170 s); warm throughput ~22 / ~28 docs/s vs dev's ~10 / ~15. Milder cold-start (2.5k cold/warm 1.25× vs dev 2.4×) and **0 antivirus-metadata timeouts** (dev's 2.5k cold hit 3) — more downstream/AV headroom, a dedicated per-tenant Mongo shard, and 2 replicas.
- **250-doc × 100 runs:** 100/100 DONE, all 250 imported, 0 failures — median **33.6 s**, p90 50 s, one 935 s load-spike outlier. Reliable at N=100.
- **5k is high-variance on this shared env** — the same 5k import ranged 196 → 1077 s across the day purely from other workloads (uploads 5 → 26 s). Large-batch timings here measure cluster load, not code; use perf for reliability/soak, or many small samples so spikes average out.

Related: [[Performance Mongo is a mongos-routed sharded cluster — truncate via the tenant's shard primary, not mongos]], [[Trust median and p90 over many runs, not the mean of a few, for latency on a noisy shared env]], [[Small import batches are overhead-bound so their per-item throughput is lower than large batches]], [[luz-docs-import cold first-import slowness is JIT plus downstream re-warm on a CPU-limited pod]].

## Related

- [[Performance Mongo is a mongos-routed sharded cluster — truncate via the tenant's shard primary, not mongos]]
- [[Trust median and p90 over many runs]]
- [[not the mean of a few]]
- [[for latency on a noisy shared env]]
- [[Small import batches are overhead-bound so their per-item throughput is lower than large batches]]
- [[luz-docs-import cold first-import slowness is JIT plus downstream re-warm on a CPU-limited pod]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import cold first-import slowness is JIT plus downstream re-warm on a CPU-limited pod]]
- [[luz-docs-import ZIP import timing fresh 100-doc ~40s vs deduped sub-second]]
- [[Small import batches are overhead-bound so their per-item throughput is lower than large batches]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]
- [[luz-docs documentscount is ~130s on an 800k tenant — the 16-shard fan-out, not counting, is the bottleneck]]

**Relations:**
- luz-docs-import — *benchmarked on* — performance env
- luz-docs-import — *benchmarked on* — dev env
- performance env — *uses code/image* — c6b1e23
- dev env — *uses code/image* — c6b1e23
- performance env — *has tenant* — 45b05710…
- benchmark — *conducted on* — 2026-08-13
- import benchmark reports — *located at* — docs/tests/import-benchmark-2026-08-13_env_performance*
- performance env — *is* — 2× faster than dev env
- performance env — *handles* — 1k warm import
- 1k warm import — *in* — 46 s
- dev env — *handles* — 1k warm import
- 1k warm import — *in* — 100 s
- performance env — *handles* — 2.5k warm import
- 2.5k warm import — *in* — 89 s
- dev env — *handles* — 2.5k warm import
- 2.5k warm import — *in* — 170 s
- performance env — *has warm throughput* — 22 docs/s
- performance env — *has warm throughput* — 28 docs/s
- dev env — *has warm throughput* — 10 docs/s
- dev env — *has warm throughput* — 15 docs/s
- performance env — *has milder* — cold-start
- performance env — *has cold/warm ratio* — 1.25×
- dev env — *has cold/warm ratio* — 2.4×
- performance env — *has* — 0 antivirus-metadata timeouts
- dev env — *hit* — 3 antivirus-metadata timeouts
- performance env — *has* — downstream/AV headroom
- performance env — *has* — dedicated per-tenant Mongo shard
- performance env — *has* — 2 replicas
- 250-doc × 100 runs — *resulted in* — 100/100 DONE
- 250-doc × 100 runs — *resulted in* — 250 imported
- 250-doc × 100 runs — *resulted in* — 0 failures
- 250-doc × 100 runs — *has median time* — 33.6 s
- 250-doc × 100 runs — *has p90 time* — 50 s
- 250-doc × 100 runs — *has outlier* — 935 s
- N=100 — *implies* — Reliable
- 5k import — *is* — high-variance
- 5k import — *on* — shared env
- 5k import — *ranged* — 196 s
- 5k import — *ranged* — 1077 s
- 5k import — *affected by* — other workloads
- other workloads — *include* — uploads
- uploads — *ranged* — 5 s
- uploads — *ranged* — 26 s
- Large-batch timings — *measure* — cluster load
- Large-batch timings — *do not measure* — code
- performance env — *used for* — reliability
- performance env — *used for* — soak
- Performance Mongo — *is a* — mongos-routed sharded cluster
- truncate — *via* — tenant's shard primary
- truncate — *not via* — mongos
- Trust — *median* — for latency
- Trust — *p90* — for latency
- Trust — *many runs* — for latency
- Do not trust — *mean* — for latency
- Do not trust — *few runs* — for latency
- latency — *on* — noisy shared env
- Small import batches — *are* — overhead-bound
- Small import batches — *have lower* — per-item throughput
- per-item throughput — *is lower than* — large batches
- luz-docs-import cold first-import slowness — *caused by* — JIT
- luz-docs-import cold first-import slowness — *caused by* — downstream re-warm
- luz-docs-import cold first-import slowness — *occurs on* — CPU-limited pod

%% ai-graph-end %%