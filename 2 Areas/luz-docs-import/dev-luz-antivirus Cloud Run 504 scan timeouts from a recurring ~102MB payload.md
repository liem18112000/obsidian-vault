---
ai_hash: e6cc5a1b43cc437e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities:
- dev-luz-antivirus
- Cloud Run service
- 504 scan timeouts
- Recurring 102 MB payload
- dev environment
- klara-nonprod project
- europe-west6 region
- GKE workload
- luz-antivirus:8080 Service
- in-cluster callers
- Cloud Run URL
- HTTP 504
- POST /luz_antivirus/api/scanner
- ClamAV
- containerConcurrency=10
- retry loop
- Saturation burst
- Small payloads (2 KB-167 KB)
- HTTP 503
- Cloud Run timeout 3600s
- Real scan deadline
- Upstream of Cloud Run
- In-cluster forwarder
- GFE
- H2C path
- minScale=1
- maxScale=58
- startup-cpu-boost
- luz-docs-import
- '*.metadata.json sidecars'
- scancenter
- luz-docs
- document binary
- Caller read-timeout
- Proxy timeout
- ClamAV StreamMaxLength
- Max scan size
- 30s AV client timeout
- 06:11-06:24 Gap-3 import test
- luz-docs-import ZIP import timing
- Recommendation 1
- Recommendation 2
- Recommendation 3
- Oversized files
- fail-fast pattern
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

- [[luz-docs-import ZIP import timing: fresh 100-doc ~40s vs deduped sub-second]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import upload-zip endpoint is the ingestion saturation point under perf load]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]
- [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]]
- [[Run volume import fixtures last; retry-exhaustion is transient saturation not a defect]]
- [[Perf import failures root-cause luz-vault sealedunready cascades jsonstore 503 to upload-zip 500]]

**Relations:**
- dev-luz-antivirus — *IS A* — Cloud Run service
- dev-luz-antivirus — *EXPERIENCES* — 504 scan timeouts
- 504 scan timeouts — *CAUSED BY* — Recurring 102 MB payload
- dev-luz-antivirus — *DEPLOYED IN* — dev environment
- dev environment — *IS PART OF* — klara-nonprod project
- klara-nonprod project — *IS IN* — europe-west6 region
- dev-luz-antivirus — *IS NOT A* — GKE workload
- in-cluster callers — *REACH* — dev-luz-antivirus
- in-cluster callers — *REACH VIA* — luz-antivirus:8080 Service
- luz-antivirus:8080 Service — *FRONTS* — Cloud Run URL
- 504 scan timeouts — *ARE* — HTTP 504
- HTTP 504 — *OCCUR ON* — POST /luz_antivirus/api/scanner
- HTTP 504 — *CAPPED AT* — 300 seconds
- Recurring 102 MB payload — *HAS* — requestSize=102473382
- Recurring 102 MB payload — *FAILS* — every 33 minutes
- Recurring 102 MB payload — *CAUSES* — 301s timeout
- Recurring 102 MB payload — *IS A* — retry loop
- ClamAV — *CANNOT PROCESS* — Recurring 102 MB payload
- ClamAV — *HAS* — deadline
- Saturation burst — *OCCURRED* — 02:59-04:24 UTC
- Saturation burst — *CAUSED* — Small payloads (2 KB-167 KB)
- Small payloads (2 KB-167 KB) — *TIMED OUT AT* — 300 seconds
- Recurring 102 MB payload — *MONOPOLISES* — containerConcurrency=10
- Saturation burst — *INCLUDED* — HTTP 503
- HTTP 503 — *INDICATES* — overload
- Cloud Run service — *HAS* — Cloud Run timeout 3600s
- Real scan deadline — *IS* — 300 seconds
- Real scan deadline — *IMPOSED BY* — Upstream of Cloud Run
- Upstream of Cloud Run — *INCLUDES* — In-cluster forwarder
- Upstream of Cloud Run — *INCLUDES* — GFE
- GFE — *HANDLES* — H2C path
- Cloud Run service — *HAS* — containerConcurrency=10
- Cloud Run service — *HAS* — minScale=1
- Cloud Run service — *HAS* — maxScale=58
- Cloud Run service — *HAS* — startup-cpu-boost
- Recommendation 1 — *IS* — find & stop 102 MB retry source
- 102 MB retry source — *IS NOT* — luz-docs-import
- luz-docs-import — *SCANS* — *.metadata.json sidecars
- 102 MB retry source — *LIKELY* — scancenter
- 102 MB retry source — *LIKELY* — luz-docs
- scancenter — *SCANS* — document binary
- luz-docs — *SCANS* — document binary
- Recommendation 2 — *IS* — Align Caller read-timeout
- Recommendation 2 — *IS* — Align Proxy timeout
- Recommendation 2 — *IS* — Align ClamAV StreamMaxLength
- Recommendation 2 — *IS* — Align Max scan size
- Oversized files — *SHOULD BE* — rejected fast
- Recommendation 3 — *IS* — luz-docs-import's new 30s AV client timeout
- 30s AV client timeout — *IS A* — fail-fast pattern
- 06:11-06:24 Gap-3 import test — *HAD* — no AV errors
- luz-docs-import ZIP import timing — *IS* — Related

%% ai-graph-end %%