---
ai_hash: 1bbbaa96da0d5c4b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities:
- dev-luz-antivirus
- Cloud Run service
- Cloud Run
- 504 scan timeouts
- 102MB payload
- dev
- klara-nonprod
- europe-west6
- GKE workload
- luz-antivirus:8080 Service/forwarder
- Cloud Run URL
- HTTP 504
- POST /luz_antivirus/api/scanner
- 300 s
- requestSize=102473382
- ClamAV
- retry loop
- containerConcurrency=10
- 503s
- Cloud Run timeoutSeconds=3600
- upstream of Cloud Run
- in-cluster forwarder
- GFE
- H2C path
- container
- minScale=1
- maxScale=58
- startup-cpu-boost
- luz-docs-import
- scancenter
- luz-docs
- document binary
- caller read-timeout
- proxy timeout
- ClamAV StreamMaxLength
- max-scan-size
- 30 s AV client timeout
- 06:11–06:24 Gap-3 import test
- 'luz-docs-import ZIP import timing: fresh 100-doc ~40s vs deduped sub-second'
source: session 2026-08-11 AV scan-timeout check
status: seedling
tags:
- luz-antivirus
- cloud-run
- clamav
- timeout
- scancenter
- dev
title: dev-luz-antivirus Cloud Run 504 scan timeouts from a recurring ~102MB payload
type: observation
---

# dev-luz-antivirus Cloud Run 504 scan timeouts from a recurring ~102MB payload

Observed 2026-08-11 on dev (`klara-nonprod` / `europe-west6`). The AV scanner **`dev-luz-antivirus` is a Cloud Run service** (not a GKE workload); in-cluster callers reach it via a `luz-antivirus:8080` Service/forwarder that fronts the Cloud Run URL.

**Symptom:** ~42 ERROR request-logs in 12h, dominated by **HTTP 504 capped at ~300 s** on `POST /luz_antivirus/api/scanner`.

Two classes:
- **Recurring ~102 MB payload** (`requestSize=102473382`, byte-identical) fails **~every 33 min**, always ~301 s → 504. The fixed size + regular cadence = a stuck **retry loop** re-submitting the same blob; ClamAV can't finish it inside the deadline.
- **Saturation burst** (~02:59–04:24): even **2 KB–167 KB** payloads timed out at 300 s. A few-KB file normally scans in ~30 ms, so the instance was jammed — the 102 MB scans monopolise the `containerConcurrency=10` slots. Also a few 503s (276 s / 375 s) = overload.

**Key config gotcha:** Cloud Run `timeoutSeconds=3600` (1 h), yet every 504 caps at **~300 s** → the real scan deadline is imposed **upstream of Cloud Run** (the in-cluster forwarder / GFE for the H2C path), *not* the container. So tuning only the Cloud Run timeout won't change the 300 s ceiling. Instance config: `containerConcurrency=10`, minScale=1, maxScale=58, startup-cpu-boost on.

**Fix direction:** (1) find & stop the ~102 MB retry source — it is NOT luz-docs-import (which only scans small `*.metadata.json` sidecars in ~30–100 ms); likely scancenter / luz-docs scanning a document binary. (2) Align caller read-timeout + proxy timeout + a ClamAV `StreamMaxLength`/max-scan-size so oversized files are **rejected fast** instead of hanging 300 s and starving concurrency. (3) luz-docs-import's new **30 s AV client timeout** (fail-fast → classify failed) is the correct pattern the 300 s-hanging callers lack.

Cross-check: no AV errors during the 06:11–06:24 Gap-3 import test — those scans were all fast 200s.

## Related

- [[luz-docs-import ZIP import timing fresh 100-doc ~40s vs deduped sub-second|luz-docs-import ZIP import timing: fresh 100-doc ~40s vs deduped sub-second]]

%% ai-graph-start %%

**Related notes:**
- [[Part B - luz-antivirus Analysis]]
- [[Service Error Analysis Report - FAILED_TO_STORE on Production]]
- [[luz-docs-import upload-zip endpoint is the ingestion saturation point under perf load]]
- [[ClamAV definition updates restart clamd, so scheduled 503s are expected not broken]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]

**Relations:**
- dev-luz-antivirus — *IS_A* — Cloud Run service
- dev-luz-antivirus — *EXPERIENCES* — 504 scan timeouts
- dev-luz-antivirus — *PROCESSES* — 102MB payload
- dev-luz-antivirus — *RUNS_ON* — dev
- dev-luz-antivirus — *RUNS_IN_PROJECT* — klara-nonprod
- dev-luz-antivirus — *RUNS_IN_REGION* — europe-west6
- dev-luz-antivirus — *ACCESSED_VIA* — luz-antivirus:8080 Service/forwarder
- dev-luz-antivirus — *HANDLES_ENDPOINT* — POST /luz_antivirus/api/scanner
- dev-luz-antivirus — *USES* — ClamAV
- dev-luz-antivirus — *HAS_CONFIG* — containerConcurrency=10
- dev-luz-antivirus — *HAS_CONFIG* — minScale=1
- dev-luz-antivirus — *HAS_CONFIG* — maxScale=58
- dev-luz-antivirus — *HAS_CONFIG* — startup-cpu-boost
- dev-luz-antivirus — *HAS_CONFIG* — Cloud Run timeoutSeconds=3600
- Cloud Run service — *IS_A* — Cloud Run
- 504 scan timeouts — *ARE* — HTTP 504
- 504 scan timeouts — *CAPPED_AT* — 300 s
- 504 scan timeouts — *CAUSED_BY* — 102MB payload
- 102MB payload — *HAS_SIZE* — requestSize=102473382
- 102MB payload — *CAUSES* — retry loop
- ClamAV — *CANNOT_FINISH* — 102MB payload
- luz-antivirus:8080 Service/forwarder — *FRONTS* — Cloud Run URL
- luz-antivirus:8080 Service/forwarder — *IMPOSES_DEADLINE* — 300 s
- luz-antivirus:8080 Service/forwarder — *IS_A* — in-cluster forwarder
- 300 s — *IMPOSED_BY* — upstream of Cloud Run
- 300 s — *IMPOSED_BY* — in-cluster forwarder
- 300 s — *IMPOSED_BY* — GFE
- retry loop — *RE_SUBMITS* — 102MB payload
- containerConcurrency=10 — *SLOTS_MONOPOLISED_BY* — 102MB payload
- 503s — *INDICATE* — overload
- GFE — *USES* — H2C path
- luz-docs-import — *SCANS* — small *.metadata.json sidecars
- luz-docs-import — *HAS* — 30 s AV client timeout
- scancenter — *LIKELY_SCANS* — document binary
- luz-docs — *LIKELY_SCANS* — document binary
- caller read-timeout — *SHOULD_ALIGN_WITH* — proxy timeout
- caller read-timeout — *SHOULD_ALIGN_WITH* — ClamAV StreamMaxLength
- proxy timeout — *SHOULD_ALIGN_WITH* — caller read-timeout
- proxy timeout — *SHOULD_ALIGN_WITH* — ClamAV StreamMaxLength
- ClamAV StreamMaxLength — *IS_A* — max-scan-size
- ClamAV StreamMaxLength — *SHOULD_ALIGN_WITH* — caller read-timeout
- ClamAV StreamMaxLength — *SHOULD_ALIGN_WITH* — proxy timeout
- 30 s AV client timeout — *IS_A* — correct pattern
- 30 s AV client timeout — *IS_FOR* — luz-docs-import
- 06:11–06:24 Gap-3 import test — *HAD* — fast 200s
- luz-docs-import — *RELATED_TO* — luz-docs-import ZIP import timing: fresh 100-doc ~40s vs deduped sub-second

%% ai-graph-end %%