---
ai_hash: 9882afbecf042e89
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities:
- luz-docs-import
- cold first-import slowness
- JIT
- downstream service re-warming
- CPU-limited 3-core Shenandoah pod
- ZIP-import run
- benchmark case
- warm runs
- documents/folders/document-import-jobs
- warm-up of the hot per-document create path
- HotSpot JIT
- batch-size discriminator
- view-controller
- jsonstore
- antivirus
- JIT threads
- GC threads
- import workers
- createDocument round-trip
- connection-pool/TLS warm-up
- REST clients
- 'Connection: close'
- Constants.java:50-51
- plaintext HTTP
- thread-pool ramp
- 16-thread executor
- Prod impact
- first import
- pod (re)start
- long idle
- A latency penalty in the tail not the median points to JIT/GC warm-up under CPU
  contention
- Truncating DB collections between benchmark runs resets data but not service warmth
- Small per-run batches warm the JIT gradually across runs; large batches warm within
  one run
- luz_docs_import upload-zip is slow for large files due to a synchronous double-write
- docs/tests/import-run1-coldstart-root-cause.md
source: docs/tests/import-run1-coldstart-root-cause.md 2026-08-13
status: seedling
tags:
- luz-docs-import
- performance
- cold-start
- jit
- LUZ-158230
title: luz-docs-import cold first-import slowness is JIT plus downstream re-warm on
  a CPU-limited pod
type: observation
---

# luz-docs-import cold first-import slowness is JIT plus downstream re-warm on a CPU-limited pod

In `luz-docs-import`, the first (cold) ZIP-import run of each benchmark case was ~1.6–2.4× slower than the warm runs **even though documents/folders/document-import-jobs are truncated before every run**. Root cause (full evidence in `docs/tests/import-run1-coldstart-root-cause.md`): **warm-up of the hot per-document create path** — HotSpot **JIT** (CONFIRMED; owns the batch-size discriminator) **+ downstream service re-warming** (view-controller / jsonstore / antivirus, re-cooled over the ~2h idle *between* cases; CONFIRMED, dominant for the big run-1 spike) — amplified by the **CPU-limited 3-core Shenandoah pod** where JIT/GC threads contend with the 16 concurrent import workers.

**Fingerprint:** the `createDocument` round-trip (the dominant cost) has a **flat median (~600–660 ms) cold vs warm but a 2–4× fatter tail when cold** (p90 3457→1110 ms, 5k run1→run2). **Ruled out:** connection-pool/TLS warm-up — all four REST clients send `Connection: close` (`Constants.java:50-51`), plaintext HTTP, no pool to populate; thread-pool ramp — a fresh, fully-sized 16-thread executor per import. Steady-state ≈ **950–980 ms/doc (~14–15 docs/s)** regardless of batch size. Prod impact: cold-start only bites the **first import after a pod (re)start or long idle**.

Related: [[A latency penalty in the tail not the median points to JITGC warm-up under CPU contention|A latency penalty in the tail not the median points to JIT/GC warm-up under CPU contention]], [[Truncating DB collections between benchmark runs resets data but not service warmth]], [[Small per-run batches warm the JIT gradually across runs; large batches warm within one run]], [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]].

## Related

- [[A latency penalty in the tail not the median points to JITGC warm-up under CPU contention|A latency penalty in the tail not the median points to JIT/GC warm-up under CPU contention]]
- [[Truncating DB collections between benchmark runs resets data but not service warmth]]
- [[Small per-run batches warm the JIT gradually across runs; large batches warm within one run]]
- [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import performance-env import benchmark findings]]
- [[Small import batches are overhead-bound so their per-item throughput is lower than large batches]]
- [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]
- [[luz-docs-import ZIP import timing fresh 100-doc ~40s vs deduped sub-second]]

**Relations:**
- luz-docs-import — *experiences* — cold first-import slowness
- cold first-import slowness — *is caused by* — JIT
- cold first-import slowness — *is caused by* — downstream service re-warming
- cold first-import slowness — *is amplified by* — CPU-limited 3-core Shenandoah pod
- ZIP-import run — *is part of* — luz-docs-import
- cold ZIP-import run — *is slower than* — warm runs
- documents/folders/document-import-jobs — *are truncated before* — every run
- warm-up of the hot per-document create path — *is root cause of* — cold first-import slowness
- warm-up of the hot per-document create path — *involves* — HotSpot JIT
- HotSpot JIT — *owns* — batch-size discriminator
- warm-up of the hot per-document create path — *involves* — downstream service re-warming
- downstream service re-warming — *includes* — view-controller
- downstream service re-warming — *includes* — jsonstore
- downstream service re-warming — *includes* — antivirus
- downstream service re-warming — *is re-cooled over* — ~2h idle
- CPU-limited 3-core Shenandoah pod — *has* — JIT threads
- CPU-limited 3-core Shenandoah pod — *has* — GC threads
- CPU-limited 3-core Shenandoah pod — *has* — import workers
- JIT threads — *contend with* — import workers
- GC threads — *contend with* — import workers
- createDocument round-trip — *is* — dominant cost
- createDocument round-trip — *has* — flat median (~600–660 ms) cold vs warm
- createDocument round-trip — *has* — 2–4× fatter tail when cold
- connection-pool/TLS warm-up — *is ruled out as cause of* — cold first-import slowness
- REST clients — *send* — Connection: close
- Connection: close — *is defined in* — Constants.java:50-51
- thread-pool ramp — *is ruled out as cause of* — cold first-import slowness
- 16-thread executor — *is* — fresh, fully-sized
- Prod impact — *affects* — first import
- first import — *occurs after* — pod (re)start
- first import — *occurs after* — long idle
- luz-docs-import — *is related to* — A latency penalty in the tail not the median points to JIT/GC warm-up under CPU contention
- luz-docs-import — *is related to* — Truncating DB collections between benchmark runs resets data but not service warmth
- luz-docs-import — *is related to* — Small per-run batches warm the JIT gradually across runs; large batches warm within one run
- luz-docs-import — *is related to* — luz_docs_import upload-zip is slow for large files due to a synchronous double-write
- full evidence — *is documented in* — docs/tests/import-run1-coldstart-root-cause.md

%% ai-graph-end %%