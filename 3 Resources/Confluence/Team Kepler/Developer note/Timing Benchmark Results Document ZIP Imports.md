---
ai_hash: 2ee476be4b461849
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49662787598'
confluence_path: Team Kepler > Developer note
created: 2026-08-13
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: Timing Benchmark Results Document ZIP Imports
type: source
updated: 2026-08-13
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49662787598/Timing+Benchmark+Results+Document+ZIP+Imports
---

# Timing Benchmark Results Document ZIP Imports

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49662787598/Timing+Benchmark+Results+Document+ZIP+Imports) · updated 2026-08-13*

## Test Data

- **Env:** performance

- **Tenant:** `45b05710-b9d4-4d3e-935e-83c4525369fa`

- **Method:** For each run — truncate `documents` + `folders` + `document-import-jobs` (cold start), `POST upload-zip`, then poll `GET …/import-jobs/{id}?showWarning=true` every 2 s until `status` = `DONE`/`FAILED`. `end_to_end_s` **= upload-start → terminal status.**

## Test Results

- **1k import: ~73–160 s end-to-end, all 5 runs clean** (1000/1000 imported). A clear **warm-up trend** — times fall run-over-run (160 s → 73 s) on the same pod.

- **2.5k import: ~160–195 s warm, all 5 runs clean** (2500/2500), with run 1 a cold outlier at 394.8 s + 3 AV timeouts.

- **5k import: ~327–371 s warm, all 5 runs clean** (5000/5000, 0 failures); cold run 1 = 622.8 s. Warm mean **344 s**, ~14.5 docs/s. (The first attempt's runs 2–5 were lost to a connectivity drop and cleanly re-run.)

- **Upload sync phase is small and scales with size** (~3.6 s for 1k, ~2.4–7 s for 2.5k, ~3.5–6.7 s for 5k) — consistent with the earlier finding that the sync path is dominated by body transfer, not server work.

- **Real service behavior surfaced:** bulk imports of many sidecars can trip the **30 s per-file antivirus timeout** on the metadata scan under load — a **variable tail** (2.5k run 1: 3; first 5k attempt: 10; 5k re-run: 0 on every run). Job still completes `DONE`; affected files land in `failedFiles`.

## Test Artifacts

- `results.csv` — raw per-run rows.

- `jobs/<file>-run<N>.json` — final job JSON (authoritative counts + `failedFiles` details).

- `jobs/<file>-run<N>-upload.json` — upload response.

- `logs/<file>-run<N>.log` — filtered pod logs per run; `logs/<file>-run<N>.clean.log` — truncation output.

[[test_data.zip|test_data.zip]][[benchmark-results-raw.zip|benchmark-results-raw.zip]]

![[image-20260813-014018.png]]

## Details Test Runs

### 1kdoc — 1000 docs (5 runs, all DONE)

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **run** | **upload_post_s** | **end_to_end_s** | **status** | **successful** | **failed** |
| 1 | 3.76 | 159.8 | DONE | 1000 | 0 |
| 2 | 3.30 | 137.0 | DONE | 1000 | 0 |
| 3 | 3.75 | 99.0 | DONE | 1000 | 0 |
| 4 | 3.99 | 91.1 | DONE | 1000 | 0 |
| 5 | 3.25 | 73.1 | DONE | 1000 | 0 |

- **end_to_end_s:** min 73.1 · max 159.8 · mean **112.0** · median **99.0**

- **upload_post_s:** min 3.25 · max 3.99 · mean **3.61** · median 3.75

- Monotonic decrease across runs → **warm-up effect** (JIT, connection pools, view-controller/jsonstore warmup on the freshly-rolled pod).

- The steady-state 1k import is nearer the **~73–99 s** end of the range than the first-run 160 s.

![[image-20260813-015550.png]]

### 2.5kdoc — 2500 docs (5 runs, all DONE)

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **run** | **upload_post_s** | **end_to_end_s** | **status** | **successful** | **failed** |
| 1 | 7.24 | 394.8 | DONE | 2497 | **3** (AV timeout) |
| 2 | 2.60 | 195.2 | DONE | 2500 | 0 |
| 3 | 2.38 | 160.7 | DONE | 2500 | 0 |
| 4 | 3.09 | 163.6 | DONE | 2500 | 0 |
| 5 | 2.46 | 162.4 | DONE | 2500 | 0 |

- **end_to_end_s:** min 160.7 · max 394.8 · mean **215.3** · median **163.6** **upload_post_s:** min 2.38 · max 7.24 · mean **3.56** · median 2.60

- Same **cold-start outlier** as the other cases: run 1 = 394.8 s with 3 AV-metadata timeouts; runs 2–5 settle to **~160–195 s, all 2500/2500 clean** (warm steady-state ≈ **165 s**). Upload sync is likewise inflated on the cold run (7.24 s) then ~2.4–3.1 s.

- Ran on a warm pod immediately after the earlier cases, which is why even run 1 here is far better than the 5k cold run.

![[image-20260813-015924.png]]

### 5kdoc — 5000 docs (5 runs, all DONE)

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **run** | **upload_post_s** | **end_to_end_s** | **status** | **successful** | **failed** |
| 1 | 6.72 | 622.8 | DONE | 5000 | 10 (AV timeout) |
| 2 | 4.71 | 332.7 | DONE | 5000 | 0 |
| 3 | 4.97 | 327.4 | DONE | 5000 | 0 |
| 4 | 6.33 | 370.6 | DONE | 5000 | 0 |
| 5 | 3.52 | 347.0 | DONE | 5000 | 0 |

- **end_to_end_s:** min 327.4 · max 622.8 · mean **400,1**· median **347.0**

- **upload_post_s:** min 3.52 · max 6.72

- Same cold-start outlier — run 1 (622.8 s) ≈ **1.8×** the warm ~344 s. Warm 5k throughput ≈ **14.5 docs/s**, in line with 2.5k (~15/s) and above 1k (~10/s): larger batches use the bounded concurrency (`IMPORT_CONCURRENCY` = 16) better.

- This re-run hit **0 AV-metadata timeouts on all 2-5 runs**

![[image-20260813-015943.png]]

## Found Issues

### Per-file antivirus-metadata timeouts (across cases)

When occur, the detail is always `"Antivirus scan of the metadata file timed out after 30s"`.

**Mechanism**: every doc with a `.metadata.json` sidecar has that sidecar scanned by luz-antivirus before creation. A 5k import fires ~5000 metadata scans (16 concurrent, `IMPORT_CONCURRENCY` default), so under the burst luz-antivirus occasionally exceeds its **30 s** per-scan timeout and those docs fail. The 1k runs (1/5 the scans) had **zero**. The job still finishes — this is a **per-file** failure, not job-level.

**Levers:** lower the concurrent thread (current 16), raise the metadata-scan timeout (current 30s).

**Cross-case tally:**

|          |          |              |              |
|----------|----------|--------------|--------------|
| **case** | **docs** | **timeouts** | **when**     |
| 1k       | 1000     | 0            | —            |
| 2.5k     | 2500     | 3            | run 1 (cold) |
| 5k       | 5000     | 10           | run 1 (cold) |

The timeouts are a **variable, load-dependent tail**, not a deterministic function of doc count: the 2.5k cold run hit 3 and the 5k attempt hit 10. When they do occur they cluster on a cold/loaded run; the job always completes `DONE` (per-file failures, not job-level). The lever is luz-antivirus headroom vs the burst of ~16 concurrent metadata scans.

![[image-20260813-021217.png]]

### Run 1 (cold) is much slower than runs 2+ (warm)

JVM JIT compilation — **CONFIRMED** (primary for the discriminator)

- The cold penalty is a **latency tail that decays with invocations**, both within a run (5k decile 2941 ms → 1089 ms) and across runs (1k 2119 ms → 969 ms). A tail that shrinks as more calls execute is the defining behavior of tiered JIT compilation (interpreter/C1 → C2) promoting the hot per-document methods as their invocation counters cross thresholds.

- **The 1k "progressive across all 5 runs" shape is the clean JIT fingerprint.** 1k ran first on the freshly-rolled pod, so the `luz-docs-import` JVM itself was genuinely cold, and 1000 invocations/run is a *slow* warm-up rate — so the curve keeps falling run-over-run (160→137→99→91→73) instead of stepping down once. A connection-pool or one-shot cache effect would produce a **step** (run1 slow, run2+ flat), not a 5-run glide.

- **The pod shape makes JIT warm-up expensive:** CPU limit **3 cores**, memory **3Gi** (request=limit), Shenandoah GC with the **compact** heuristic (env in `luz_kubernetes` base `kubernetes/luz-docs-import/k8s.yaml`, per `docs/tests/luz-docs-import-perf-vs-prod-spec-comparison.md:88`). During the cold burst the JIT-compiler threads and GC threads compete with the 16 import workers (`DocsImportAsyncService.java:40`) for only 3 cores — so cold compilation shows up as stalled requests, exactly the fat p90.

*Nuance:* by the time the **large** cases ran (2–2.3 h after 1k, same never-restarted JVM), `luz-docs-import`'s own app code was already C2-compiled from tens of thousands of prior invocations. So the within-5k-run1 decline is **downstream** JIT/warm-up (next hypothesis), not app-JVM JIT. JIT is confirmed as the mechanism that explains the *discriminator* (warm-up invocations reached faster by bigger batches, at whichever layer is cold); app-JVM JIT specifically owns the 1k-first-case progressive curve.

![[image-20260813-020202.png]]

%% ai-graph-start %%

**Related notes:**
- [[ePost Zip-Import - dev test-suite results - 18-08-2026]]
- [[Create Document API – Performance Testing Report]]
- [[Measure create API - investigate performance]]
- [[ePost ZIP-import — dev test-suite results - 13-08-2026]]
- [[luz-docs-import performance-env import benchmark findings]]

%% ai-graph-end %%