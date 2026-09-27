---
ai_hash: 36095aea6710ed4c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities:
- luz-docs-import upload-zip endpoint
- ingestion saturation point
- perf load
- performance env
- k6 load test
- 100 VUs
- 100 RPS
- 10000 iters
- commit 6f35eeb
- '2026-08-24'
- POST /luz_docs_import/api/{tenant}/import-jobs/upload-zip
- luz-docs-import
- luz-docs
- luz-docs-batch
- luz-docs-view-controller-batch
- luz-jsonstore
- HPAs
- Client timeout
- HTTP 500
- 60 s HTTP timeout
- upload-zip
- job id
- ingestion layer
- bottleneck
- downstream enrichment
- Analyze-API 429 backlog
- luz-docs-import HPA
- LUZ-158230 docs-import performance
- 60-min run
- HTTP 503
- liveness-probe death spiral
- import pods
- SIGKILLed
- exit 137
- GET /app-health/luz-docs-import/livez
- worker thread
- pool
- blocked uploads
- CPU
- 3-core limit
- I/O
- 'Liveness-probe death spiral: killing a thread-pool-saturated pod turns overload
  into a self-perpetuating outage'
- dedicated health thread
- relax thresholds
- luz-vault
- sealed/unready
- jsonstore addOne
- 'Perf import failures root-cause: luz-vault sealed/unready cascades jsonstore 503
  to upload-zip 500'
source: session 2026-08-24
status: seedling
tags:
- luz-docs-import
- performance
- k6
- LUZ-158230
- bottleneck
title: luz-docs-import upload-zip endpoint is the ingestion saturation point under
  perf load
type: observation
---

# luz-docs-import upload-zip endpoint is the ingestion saturation point under perf load

On the performance env k6 load test (100 VUs, 100 RPS, 10000 iters, commit 6f35eeb, 2026-08-24), the **synchronous `POST /luz_docs_import/api/{tenant}/import-jobs/upload-zip`** endpoint on `luz-docs-import` is the saturation point — even after scaling the whole chain (HPAs pinned: `luz-docs-import` 2/2, `luz-docs` 5, `luz-docs-batch` 5, `luz-docs-view-controller-batch` 4, `luz-jsonstore` 5).

Two failure signatures under load:

1. **Client timeout** — k6 hits its own **60 s HTTP timeout**: `status: 0, duration ~60000ms, error: request timeout`. The upload-zip request never returns in time.
2. **HTTP 500** after **33–56 s**: body `{"createdTime":...,"uuid":...,"message":""}`.

**Why it matters:** upload-zip is meant to be async (return a job id, then poll), but its *synchronous* receive → store zip → create-import-job work queues past 60 s under concurrency, so requests either time out client-side or 500. This is the **ingestion layer** and is a *distinct* bottleneck from the downstream enrichment / Analyze-API 429 backlog seen in earlier runs.

**Likely constraint:** `luz-docs-import` HPA was pinned at **max 2** — that tier cannot absorb 100 RPS of zip uploads. Next step: raise `luz-docs-import` max, and/or check whether it is CPU/throttle-bound during the run before concluding it is pure concurrency.

Related: [[LUZ-158230 docs-import performance]]

## Related

- [[LUZ-158230 docs-import performance]]

## Confirmed outcome — run complete (2026-08-24)

The full 60-min run **failed 100%**: 0/8784 upload-zip requests returned 200. Failure mix: **61% 60s client-timeouts (status 0), 28% HTTP 503 (bulkhead), 11% HTTP 500**. Effective throughput ~2.4 iters/s vs a 100-RPS target; 1186 iterations dropped.

**Confirmed root cause (not just "max 2"):** a **liveness-probe death spiral** — both import pods were SIGKILLed 5x each (exit 137) because `GET /app-health/luz-docs-import/livez` could not get a worker thread while the pool was full of blocked uploads. CPU stayed ~55m of a 3-core limit (blocked on I/O, not compute), so scaling luz-docs/-batch/jsonstore/vc-batch did nothing. See the general pattern: [[Liveness-probe death spiral killing a thread-pool-saturated pod turns overload into a self-perpetuating outage|Liveness-probe death spiral: killing a thread-pool-saturated pod turns overload into a self-perpetuating outage]].

**Fix order:** repair liveness first (dedicated health thread / relax thresholds), then raise import HPA max to 5-8, then make upload-zip truly async. Full report: `docs/tests/perf-k6-loadtest-2026-08-24/`.

## SUPERSEDED as *primary* cause (2026-08-24, same day)

The fast-500 follow-up (10 VUs) showed the real primary blocker is **luz-vault being sealed/unready on performance**, cascading jsonstore `addOne` 503 → 400 → import 500. The saturation/liveness death-spiral in this note is a *secondary*, high-load-only amplifier. Primary root cause: [[Perf import failures root-cause luz-vault sealedunready cascades jsonstore 503 to upload-zip 500|Perf import failures root-cause: luz-vault sealed/unready cascades jsonstore 503 to upload-zip 500]].

%% ai-graph-start %%

**Related notes:**
- [[Perf import failures root-cause luz-vault sealedunready cascades jsonstore 503 to upload-zip 500]]
- [[Run volume import fixtures last; retry-exhaustion is transient saturation not a defect]]
- [[luz-docs-import cold first-import slowness is JIT plus downstream re-warm on a CPU-limited pod]]
- [[Liveness-probe death spiral killing a thread-pool-saturated pod turns overload into a self-perpetuating outage]]
- [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]]

**Relations:**
- luz-docs-import upload-zip endpoint — *is* — ingestion saturation point
- luz-docs-import upload-zip endpoint — *occurs under* — perf load
- perf load — *tested in* — performance env
- performance env — *uses* — k6 load test
- k6 load test — *configured with* — 100 VUs
- k6 load test — *configured with* — 100 RPS
- k6 load test — *configured with* — 10000 iters
- k6 load test — *run on* — 2026-08-24
- k6 load test — *run with commit* — 6f35eeb
- POST /luz_docs_import/api/{tenant}/import-jobs/upload-zip — *is* — luz-docs-import upload-zip endpoint
- luz-docs-import — *hosts* — POST /luz_docs_import/api/{tenant}/import-jobs/upload-zip
- HPAs — *pinned for* — luz-docs-import
- HPAs — *pinned for* — luz-docs
- HPAs — *pinned for* — luz-docs-batch
- HPAs — *pinned for* — luz-docs-view-controller-batch
- HPAs — *pinned for* — luz-jsonstore
- POST /luz_docs_import/api/{tenant}/import-jobs/upload-zip — *shows failure signature* — Client timeout
- POST /luz_docs_import/api/{tenant}/import-jobs/upload-zip — *shows failure signature* — HTTP 500
- Client timeout — *is* — 60 s HTTP timeout
- upload-zip — *is meant to be* — async
- upload-zip — *is part of* — ingestion layer
- ingestion layer — *is a* — bottleneck
- bottleneck — *is distinct from* — downstream enrichment
- bottleneck — *is distinct from* — Analyze-API 429 backlog
- luz-docs-import HPA — *was pinned at* — max 2
- luz-docs-import HPA — *cannot absorb* — 100 RPS
- luz-docs-import HPA — *next step is to raise* — max
- luz-docs-import upload-zip endpoint — *is related to* — LUZ-158230 docs-import performance
- 60-min run — *failed* — 100%
- 60-min run — *resulted in* — 0/8784 upload-zip requests returned 200
- 60-min run — *failure mix included* — 61% 60s client-timeouts
- 60-min run — *failure mix included* — 28% HTTP 503
- 60-min run — *failure mix included* — 11% HTTP 500
- 60-min run — *had effective throughput* — ~2.4 iters/s
- 60-min run — *dropped* — 1186 iterations
- liveness-probe death spiral — *is* — Confirmed root cause
- liveness-probe death spiral — *caused* — import pods SIGKILLed
- import pods — *were* — SIGKILLed
- SIGKILLed — *with* — exit 137
- liveness-probe death spiral — *occurred because* — GET /app-health/luz-docs-import/livez could not get a worker thread
- worker thread — *was unavailable because* — pool was full of blocked uploads
- luz-docs-import — *CPU stayed* — ~55m of a 3-core limit
- luz-docs-import — *was blocked on* — I/O
- liveness-probe death spiral — *is a general pattern* — Liveness-probe death spiral: killing a thread-pool-saturated pod turns overload into a self-perpetuating outage
- liveness — *repair via* — dedicated health thread
- liveness — *repair via* — relax thresholds
- upload-zip — *should be made* — truly async
- luz-vault — *is* — sealed/unready
- luz-vault sealed/unready — *is* — real primary blocker
- luz-vault sealed/unready — *cascades to* — jsonstore addOne
- jsonstore addOne — *results in* — HTTP 503
- HTTP 503 — *cascades to* — HTTP 500
- liveness-probe death spiral — *is a* — secondary, high-load-only amplifier
- Perf import failures root-cause: luz-vault sealed/unready cascades jsonstore 503 to upload-zip 500 — *is* — Primary root cause

%% ai-graph-end %%