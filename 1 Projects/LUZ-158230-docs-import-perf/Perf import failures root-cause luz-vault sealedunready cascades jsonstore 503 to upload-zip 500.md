---
ai_hash: 554cecffd1ec90e4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities:
- Performance import failures
- luz-vault
- luz-jsonstore
- upload-zip
- k6 load test
- luz-docs-import
- luz-vault containers
- Vault health endpoint
- luz-vault-unseal-0
- HTTP 503
- document-import-jobs/add
- Vault transit crypto
- VaultException
- HTTP 400
- import service
- DocsResponseExceptionMapper
- DocsException
- UnexpectedExceptionMapper
- HTTP 500
- 10 VUs
- 100 VUs
- import worker pool
- k6 timeout
- Vault call
- saturation
- liveness death-spiral
- Vault outage
- LOW concurrency
- underlying correctness error
- request thread tracing
- import access log
- import REST-client filters
- jsonstore SEVERE logs
- vault /sys/health
- infra/Vault-owner
- app-team kubectl scope
- root-cause-CORRECTION-vault.md
- luz-docs-import upload-zip endpoint is the ingestion saturation point under perf
  load
- 'Liveness-probe death spiral: killing a thread-pool-saturated pod turns overload
  into a self-perpetuating outage'
- 10-VU re-run
- luz-vault-0/1
- build fcdea5a3
- pod -b6628
- k6 checks
- upload-zip status 200
- http_req_failed metric
- import_e2e_duration_ms metric
- import_upload_duration_ms metric
- 10 RPS
- 1000-request case
- capacity ceiling
- AV limits
- Vault healthy
source: session 2026-08-24
status: seedling
tags:
- luz-vault
- luz-jsonstore
- luz-docs-import
- performance
- root-cause
- LUZ-158230
- cascade-failure
title: 'Perf import failures root-cause: luz-vault sealed/unready cascades jsonstore
  503 to upload-zip 500'
type: observation
---

# Perf import failures root-cause: luz-vault sealed/unready cascades jsonstore 503 to upload-zip 500

On performance (2026-08-24), the k6 import load test failed 100% because **`luz-vault` is sealed / not-ready**, not because of anything in luz-docs-import. Both `luz-vault-*` containers report `ready=false` (since ~23 Aug), `/v1/sys/health?standbyok=true` returns **503**, and `luz-vault-unseal-0` has restarted 7x.

**Cascade:** Vault 503 → `luz-jsonstore` `document-import-jobs/add` (addOne) needs Vault transit crypto → throws `VaultException: ... status code 503` → jsonstore returns **HTTP 400** to import → import `DocsResponseExceptionMapper` maps 400 → `DocsException` → `UnexpectedExceptionMapper` → **HTTP 500** on `upload-zip`.

**Load-dependent disguise:** at **10 VUs** the failure shows through *fast* (~100ms, HTTP 500). At **100 VUs** the import worker pool saturates and requests queue to the 60s k6 timeout *before* reaching the Vault call, so it looked like a saturation/liveness death-spiral instead. Both are real, but Vault-down is the PRIMARY blocker; saturation is a high-load amplifier. See [[Liveness-probe death spiral: killing a thread-pool-saturated pod turns overload into a self-perpetuating outage]].

**Diagnostic technique that worked:** re-run at LOW concurrency to strip away saturation and expose the underlying correctness error; then trace one request thread top-to-bottom across services (import access log -> import REST-client filters -> jsonstore SEVERE -> vault /sys/health) to reach the bottom of the stack.

**Fix:** unseal/repair luz-vault on performance (investigate crash-looping `luz-vault-unseal-0`) — infra/Vault-owner action, likely outside app-team kubectl scope. Re-run only after `luz-vault-*` are 2/2 Ready and `/sys/health`=200. Full report: `docs/tests/perf-k6-loadtest-2026-08-24/root-cause-CORRECTION-vault.md`.

## Related

- [[luz-docs-import upload-zip endpoint is the ingestion saturation point under perf load]]
- [[Liveness-probe death spiral: killing a thread-pool-saturated pod turns overload into a self-perpetuating outage]]
## ✅ CONFIRMED — 10-VU re-run after Vault recovered = 100% pass

After `luz-vault-0/1` came back to **2/2 Ready** (`ready=true`), the identical 10-VU / 10-RPS / 1000-request case was re-run (build `fcdea5a3…`, pod `…-b6628`) and **passed completely**:

- **checks: 100.00% (7000/7000)** — all 7 assertions green: `upload-zip status is 200`, reached terminal in time, status/successfulFiles/skippedFiles/failedFiles/rejectedFiles all match expected.
- **http_req_failed: 0.00%** (0/3083); 1000/1000 iterations, 0 interrupted.
- **import_e2e_duration_ms**: avg 6409, med 6064, p90 12.1s, p95 15.1s, p99 18.4s, max 21.4s.
- **import_upload_duration_ms**: avg 85, med 37, p95 125, p99 1153, max 4888.
- Run wall-clock ~11m at 10 VUs (vus 3–10), data_sent 183 MB.

This closes the loop: the 100% failure in both prior runs was **entirely** the Vault outage. With Vault healthy, the import path is fully functional at 10 VUs. Next step is to ramp back toward 100 VUs to find the *real* capacity ceiling (which may then surface genuine import saturation / AV limits).

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import upload-zip endpoint is the ingestion saturation point under perf load]]
- [[Run volume import fixtures last; retry-exhaustion is transient saturation not a defect]]
- [[luz-docs 2026-06-11 dev integration run failure clusters]]
- [[luz-docs-import cold first-import slowness is JIT plus downstream re-warm on a CPU-limited pod]]
- [[luz-docs-import performance-env import benchmark findings]]

**Relations:**
- Performance import failures — *root-cause* — luz-vault
- luz-vault — *state* — sealed / not-ready
- luz-vault — *cascades* — luz-jsonstore
- luz-jsonstore — *cascades* — upload-zip
- k6 load test — *failed* — 100%
- k6 load test — *targets* — luz-docs-import
- luz-vault containers — *report* — ready=false
- Vault health endpoint — *returns* — HTTP 503
- luz-vault-unseal-0 — *restarted* — 7x
- luz-jsonstore — *needs* — Vault transit crypto
- document-import-jobs/add — *throws* — VaultException
- VaultException — *has status code* — HTTP 503
- luz-jsonstore — *returns* — HTTP 400
- HTTP 400 — *to* — import service
- import service — *uses* — DocsResponseExceptionMapper
- DocsResponseExceptionMapper — *maps* — HTTP 400
- DocsResponseExceptionMapper — *maps to* — DocsException
- DocsException — *handled by* — UnexpectedExceptionMapper
- UnexpectedExceptionMapper — *results in* — HTTP 500
- HTTP 500 — *on* — upload-zip
- 10 VUs — *reveals failure* — fast
- 100 VUs — *saturates* — import worker pool
- import worker pool — *causes* — requests queue
- requests queue — *exceeds* — k6 timeout
- Vault outage — *is* — PRIMARY blocker
- saturation — *is* — high-load amplifier
- LOW concurrency — *exposes* — underlying correctness error
- request thread tracing — *diagnoses* — underlying correctness error
- request thread tracing — *involves* — import access log
- request thread tracing — *involves* — import REST-client filters
- request thread tracing — *involves* — jsonstore SEVERE logs
- request thread tracing — *involves* — vault /sys/health
- Fix — *is* — unseal/repair luz-vault
- unseal/repair luz-vault — *is* — infra/Vault-owner action
- infra/Vault-owner — *is outside* — app-team kubectl scope
- Full report — *located at* — root-cause-CORRECTION-vault.md
- luz-docs-import upload-zip endpoint is the ingestion saturation point under perf load — *related to* — upload-zip
- Liveness-probe death spiral: killing a thread-pool-saturated pod turns overload into a self-perpetuating outage — *related to* — liveness death-spiral
- 10-VU re-run — *passed* — 100%
- 10-VU re-run — *occurred after* — Vault healthy
- luz-vault-0/1 — *state* — 2/2 Ready
- 10-VU re-run — *used build* — build fcdea5a3
- 10-VU re-run — *used pod* — pod -b6628
- 10-VU re-run — *resulted in* — k6 checks: 100.00%
- k6 checks — *includes* — upload-zip status 200
- 10-VU re-run — *resulted in* — http_req_failed metric: 0.00%
- 10-VU re-run — *measured* — import_e2e_duration_ms metric
- 10-VU re-run — *measured* — import_upload_duration_ms metric
- 10-VU re-run — *used* — 10 VUs
- 10-VU re-run — *used* — 10 RPS
- 10-VU re-run — *is* — 1000-request case
- Vault outage — *caused* — 100% failure
- Vault healthy — *enables* — import service functional
- Next step — *is* — ramp toward 100 VUs
- ramp toward 100 VUs — *to find* — capacity ceiling
- capacity ceiling — *may surface* — saturation
- capacity ceiling — *may surface* — AV limits

%% ai-graph-end %%