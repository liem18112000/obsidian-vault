---
ai_hash: 52ad415a9dd5a6e4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities:
- luz-docs-import
- performance env
- dev env
- c6b1e23
- 45b05710…
- ZIP import
- 1k warm import
- 2.5k warm import
- 250-doc × 100 runs
- 5k import
- antivirus-metadata timeouts
- Mongo shard
- 2 replicas
- cluster load
- code performance
- reliability
- soak testing
- Performance Mongo
- mongos-routed sharded cluster
- tenant's shard primary
- mongos
- median
- p90
- mean
- latency
- noisy shared env
- Small import batches
- overhead-bound
- per-item throughput
- large batches
- cold first-import slowness
- JIT
- downstream re-warm
- CPU-limited pod
- warm throughput
- cold-start
- load-spike outlier
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
- luz-docs-import — *benchmarked_on* — performance env
- performance env — *uses_code_image* — c6b1e23
- dev env — *uses_code_image* — c6b1e23
- performance env — *has_tenant* — 45b05710…
- performance env — *is_faster_than* — dev env
- performance env — *has* — 0 antivirus-metadata timeouts
- dev env — *experienced* — antivirus-metadata timeouts
- performance env — *has* — dedicated per-tenant Mongo shard
- performance env — *has* — 2 replicas
- 1k warm import — *takes_on* — ~46 s
- 1k warm import — *takes_on_dev_env* — ~100 s
- 2.5k warm import — *takes_on* — ~89 s
- 2.5k warm import — *takes_on_dev_env* — ~170 s
- warm throughput — *is_on* — performance env
- warm throughput — *is_on* — dev env
- cold-start — *is_on* — performance env
- cold-start — *is_on* — dev env
- 250-doc × 100 runs — *resulted_in* — 100/100 DONE
- 250-doc × 100 runs — *had_median* — 33.6 s
- 250-doc × 100 runs — *had_p90* — 50 s
- 250-doc × 100 runs — *had* — one 935 s load-spike outlier
- 5k import — *is_on* — high-variance
- 5k import — *ranged_on* — shared env
- Large-batch timings — *measure* — cluster load
- Large-batch timings — *do_not_measure* — code performance
- performance env — *used_for* — reliability
- performance env — *used_for* — soak testing
- Performance Mongo — *is_a* — mongos-routed sharded cluster
- truncate — *via* — tenant's shard primary
- truncate — *not_via* — mongos
- Trust — *median* — true
- Trust — *p90* — true
- median — *preferred_over* — mean
- p90 — *preferred_over* — mean
- median — *is_for* — latency
- p90 — *is_for* — latency
- latency — *occurs_on* — noisy shared env
- Small import batches — *are* — overhead-bound
- Small import batches — *has_lower_throughput_than* — large batches
- luz-docs-import cold first-import slowness — *is_due_to* — JIT
- luz-docs-import cold first-import slowness — *is_due_to* — downstream re-warm
- cold first-import slowness — *occurs_on* — CPU-limited pod

%% ai-graph-end %%