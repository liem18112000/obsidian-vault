---
ai_hash: b93b9b1e57dd9a2c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities:
- luz-docs-import
- upload-zip endpoint
- POST /luz_docs_import/api/{tenant}/import-jobs/upload-zip
- k6
- performance env
- luz-docs
- luz-docs-batch
- luz-docs-view-controller-batch
- luz-jsonstore
- HPA
- Client timeout
- HTTP 500
- HTTP 503
- ingestion layer
- enrichment / Analyze-API 429 backlog
- liveness-probe death spiral
- GET /app-health/luz-docs-import/livez
- luz-vault
- jsonstore addOne
- LUZ-158230 docs-import performance
- 'Perf import failures root-cause: luz-vault sealed/unready cascades jsonstore 503
  to upload-zip 500'
- dedicated health thread
- relax thresholds
- async upload-zip
- full report
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
- [[Liveness-probe death spiral killing a thread-pool-saturated pod turns overload into a self-perpetuating outage]]
- [[luz-docs-import cold first-import slowness is JIT plus downstream re-warm on a CPU-limited pod]]
- [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]]

**Relations:**
- luz-docs-import — *hosts* — upload-zip endpoint
- upload-zip endpoint — *is* — ingestion saturation point
- upload-zip endpoint — *is* — synchronous
- POST /luz_docs_import/api/{tenant}/import-jobs/upload-zip — *is* — upload-zip endpoint
- k6 — *performs load test on* — upload-zip endpoint
- k6 — *performs load test in* — performance env
- luz-docs-import — *is scaled by* — HPA
- luz-docs — *is scaled* — HPA
- luz-docs-batch — *is scaled* — HPA
- luz-docs-view-controller-batch — *is scaled* — HPA
- luz-jsonstore — *is scaled* — HPA
- Client timeout — *is failure signature for* — upload-zip endpoint
- HTTP 500 — *is failure signature for* — upload-zip endpoint
- HTTP 503 — *is failure signature for* — upload-zip endpoint
- upload-zip endpoint — *is part of* — ingestion layer
- ingestion layer — *is bottleneck* — upload-zip endpoint
- ingestion layer — *is distinct from* — enrichment / Analyze-API 429 backlog
- luz-docs-import HPA — *was pinned at* — max 2
- liveness-probe death spiral — *is confirmed root cause for* — upload-zip endpoint
- luz-docs-import — *pods are SIGKILLed due to* — liveness-probe death spiral
- GET /app-health/luz-docs-import/livez — *is liveness probe for* — luz-docs-import
- luz-docs-import — *is blocked on* — I/O
- liveness-probe death spiral — *is general pattern* — Liveness-probe death spiral: killing a thread-pool-saturated pod turns overload into a self-perpetuating outage
- repair liveness — *is fix for* — liveness-probe death spiral
- raise import HPA max — *is fix for* — luz-docs-import
- make upload-zip truly async — *is fix for* — upload-zip endpoint
- luz-vault — *is primary blocker* — Perf import failures root-cause: luz-vault sealed/unready cascades jsonstore 503 to upload-zip 500
- luz-vault — *is* — sealed/unready
- luz-vault sealed/unready — *cascades to* — jsonstore addOne
- jsonstore addOne — *results in* — HTTP 503
- jsonstore addOne — *cascades to* — luz-docs-import
- luz-docs-import — *results in* — HTTP 500
- liveness-probe death spiral — *is secondary amplifier* — upload-zip endpoint
- Perf import failures root-cause: luz-vault sealed/unready cascades jsonstore 503 to upload-zip 500 — *is primary root cause for* — upload-zip endpoint
- LUZ-158230 docs-import performance — *is related to* — luz-docs-import
- docs/tests/perf-k6-loadtest-2026-08-24/ — *contains* — full report

%% ai-graph-end %%