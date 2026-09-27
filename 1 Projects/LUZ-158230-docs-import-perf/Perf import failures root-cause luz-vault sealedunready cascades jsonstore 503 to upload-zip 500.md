---
ai_hash: a2d0abe04a7dfc61
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities:
- Perf import failures
- luz-vault
- jsonstore
- upload-zip
- k6 import load test
- luz-docs-import
- HTTP 503
- HTTP 400
- HTTP 500
- VaultException
- DocsResponseExceptionMapper
- DocsException
- UnexpectedExceptionMapper
- transit crypto
- document-import-jobs/add
- addOne
- 10 VUs
- 100 VUs
- import worker pool
- Vault call
- saturation
- liveness death-spiral
- Vault outage
- Diagnostic technique
- low concurrency
- request thread trace
- import access log
- import REST-client filters
- jsonstore SEVERE
- vault /sys/health
- luz-vault-unseal-0
- infra/Vault-owner
- app-team kubectl scope
- performance environment
- Liveness-probe death spiral killing a thread-pool-saturated pod turns overload into
  a self-perpetuating outage
- luz-docs-import upload-zip endpoint is the ingestion saturation point under perf
  load
- Vault recovery
- import path
- capacity ceiling
- AV limits
- ready=false
- ready=true
- /v1/sys/health?standbyok=true
- docs/tests/perf-k6-loadtest-2026-08-24/root-cause-CORRECTION-vault.md
- 60s k6 timeout
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

**Load-dependent disguise:** at **10 VUs** the failure shows through *fast* (~100ms, HTTP 500). At **100 VUs** the import worker pool saturates and requests queue to the 60s k6 timeout *before* reaching the Vault call, so it looked like a saturation/liveness death-spiral instead. Both are real, but Vault-down is the PRIMARY blocker; saturation is a high-load amplifier. See [[Liveness-probe death spiral killing a thread-pool-saturated pod turns overload into a self-perpetuating outage|Liveness-probe death spiral: killing a thread-pool-saturated pod turns overload into a self-perpetuating outage]].

**Diagnostic technique that worked:** re-run at LOW concurrency to strip away saturation and expose the underlying correctness error; then trace one request thread top-to-bottom across services (import access log -> import REST-client filters -> jsonstore SEVERE -> vault /sys/health) to reach the bottom of the stack.

**Fix:** unseal/repair luz-vault on performance (investigate crash-looping `luz-vault-unseal-0`) — infra/Vault-owner action, likely outside app-team kubectl scope. Re-run only after `luz-vault-*` are 2/2 Ready and `/sys/health`=200. Full report: `docs/tests/perf-k6-loadtest-2026-08-24/root-cause-CORRECTION-vault.md`.

## Related

- [[luz-docs-import upload-zip endpoint is the ingestion saturation point under perf load]]
- [[Liveness-probe death spiral killing a thread-pool-saturated pod turns overload into a self-perpetuating outage|Liveness-probe death spiral: killing a thread-pool-saturated pod turns overload into a self-perpetuating outage]]
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
- [[Service Error Analysis Report - FAILED_TO_STORE on Production]]
- [[Part A - luz-jsonstore Analysis]]
- [[Run volume import fixtures last; retry-exhaustion is transient saturation not a defect]]
- [[Case Report - DocumentId 386]]

**Relations:**
- k6 import load test — *failed 100% on* — performance environment
- k6 import load test — *failed due to* — luz-vault
- luz-vault — *is* — sealed / not-ready
- luz-vault — *reports* — ready=false
- /v1/sys/health?standbyok=true — *returns* — HTTP 503
- luz-vault-unseal-0 — *has restarted* — 7x
- HTTP 503 — *from* — luz-vault
- HTTP 503 — *cascades to* — jsonstore
- jsonstore — *needs* — transit crypto
- jsonstore — *calls* — document-import-jobs/add
- document-import-jobs/add — *calls* — addOne
- jsonstore — *throws* — VaultException
- VaultException — *has status code* — HTTP 503
- jsonstore — *returns* — HTTP 400
- jsonstore — *returns HTTP 400 to* — luz-docs-import
- luz-docs-import — *uses* — DocsResponseExceptionMapper
- DocsResponseExceptionMapper — *maps* — HTTP 400
- DocsResponseExceptionMapper — *maps HTTP 400 to* — DocsException
- DocsException — *handled by* — UnexpectedExceptionMapper
- UnexpectedExceptionMapper — *results in* — HTTP 500
- UnexpectedExceptionMapper — *results in HTTP 500 on* — upload-zip
- 10 VUs — *shows failure as* — HTTP 500
- 100 VUs — *saturates* — import worker pool
- import worker pool — *causes* — requests queue
- requests queue — *reaches* — 60s k6 timeout
- Vault outage — *is* — PRIMARY blocker
- saturation — *is* — high-load amplifier
- saturation — *can lead to* — liveness death-spiral
- Diagnostic technique — *is* — re-run at low concurrency
- Diagnostic technique — *is* — request thread trace
- request thread trace — *involves* — import access log
- request thread trace — *involves* — import REST-client filters
- request thread trace — *involves* — jsonstore SEVERE
- request thread trace — *involves* — vault /sys/health
- Fix — *is* — unseal/repair luz-vault
- Fix — *involves* — investigate crash-looping luz-vault-unseal-0
- Fix — *is* — infra/Vault-owner action
- Fix — *is outside* — app-team kubectl scope
- luz-vault — *must be* — ready=true
- luz-vault — *must be ready=true for* — re-run
- /v1/sys/health?standbyok=true — *must return* — 200
- /v1/sys/health?standbyok=true — *must return 200 for* — re-run
- Full report — *is at* — docs/tests/perf-k6-loadtest-2026-08-24/root-cause-CORRECTION-vault.md
- Perf import failures — *related to* — luz-docs-import upload-zip endpoint is the ingestion saturation point under perf load
- Perf import failures — *related to* — Liveness-probe death spiral killing a thread-pool-saturated pod turns overload into a self-perpetuating outage
- Vault recovery — *enabled* — 10-VU re-run
- 10-VU re-run — *passed 100%* — 
- Vault outage — *caused* — 100% failure
- import path — *is functional at* — 10 VUs
- Next step — *is* — ramp toward 100 VUs
- Next step — *is* — find capacity ceiling
- capacity ceiling — *may surface* — saturation
- capacity ceiling — *may surface* — AV limits

%% ai-graph-end %%