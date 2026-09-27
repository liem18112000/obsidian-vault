---
ai_hash: f0ea008dfc8368ff
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities:
- luz-docs-import
- ZIP import timing
- 100-doc sample zips
- 40s
- sub-second
- dev tenant d0783310
- import-job Mongo document
- lastModifield
- createdAt
- per-import duration
- cross-service pipeline
- unzip
- '*.metadata.json-sidecar antivirus scan'
- jsonstore
- vault
- Mongo document writes
- folder writes
- FLUSH_EVERY_N
- FLUSH_EVERY_MS
- Fresh 100-doc import
- LEGACY
- BASELINE
- Fully-deduped / skipped run
- NFC dedup keys
- API poll
- WAY1 poll
- GET /import-jobs/{id}
- importZipName
- relativePath
- dedup identity
- Mongo delta
- ZIP entry names decode as CP437 mojibake when the UTF-8 EFS flag is unset
- uploaded filename minus extension
- existing docs
- tiny jobs
- skip runs
source: session 2026-08-11 Gap-3 + dedup dev test
status: seedling
tags:
- luz-docs-import
- performance
- mongodb
- zip-import
title: 'luz-docs-import ZIP import timing: fresh 100-doc ~40s vs deduped sub-second'
type: observation
---

# luz-docs-import ZIP import timing: fresh 100-doc ~40s vs deduped sub-second

Measured on dev (tenant d0783310…, 100-doc sample zips). The authoritative per-import duration is the import-job Mongo document's **`lastModifield − createdAt`**, which spans the whole cross-service pipeline the job drives (unzip → per-`*.metadata.json`-sidecar antivirus scan → jsonstore/vault → Mongo document + folder writes, under `FLUSH_EVERY_N=1000` / `FLUSH_EVERY_MS=10000` batching).

- **Fresh 100-doc import ≈ 40 s** (LEGACY 40.13 s, BASELINE 39.95 s) → ~0.4 s/doc, strikingly consistent.
- **Fully-deduped / skipped run = sub-second (0.35–0.72 s)** — no documents are created; the server only computes NFC dedup keys and matches existing docs, so `skipped=100 ok=0` returns almost instantly.

**Trust the Mongo delta, not the API poll for short jobs.** The test's WAY1 poll (`GET /import-jobs/{id}`) samples every 3 s with a floor, so it *overstates* tiny jobs — it reports "5 s" for a job that Mongo shows took 0.35 s. For fresh ~40 s imports the two agree; for skip runs only `lastModifield − createdAt` is accurate.

Context: dedup identity is `(importZipName, relativePath)` with `importZipName` = uploaded filename minus extension; re-uploading the same zip under the same name skips everything.

## Related

- [[ZIP entry names decode as CP437 mojibake when the UTF-8 EFS flag is unset]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import performance-env import benchmark findings]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]
- [[luz-docs-import cold first-import slowness is JIT plus downstream re-warm on a CPU-limited pod]]
- [[Small import batches are overhead-bound so their per-item throughput is lower than large batches]]
- [[luz_docs_import dedup folders via view-controller API, documents via import job history]]

**Relations:**
- luz-docs-import — *measures* — ZIP import timing
- ZIP import timing — *for* — 100-doc sample zips
- 100-doc sample zips — *takes (fresh)* — 40s
- 100-doc sample zips — *takes (deduped)* — sub-second
- ZIP import timing — *measured on* — dev tenant d0783310
- import-job Mongo document — *has field* — lastModifield
- import-job Mongo document — *has field* — createdAt
- per-import duration — *is calculated by* — lastModifield − createdAt
- per-import duration — *spans* — cross-service pipeline
- cross-service pipeline — *includes* — unzip
- cross-service pipeline — *includes* — *.metadata.json-sidecar antivirus scan
- cross-service pipeline — *includes* — jsonstore
- cross-service pipeline — *includes* — vault
- cross-service pipeline — *includes* — Mongo document writes
- cross-service pipeline — *includes* — folder writes
- cross-service pipeline — *uses batching parameter* — FLUSH_EVERY_N
- cross-service pipeline — *uses batching parameter* — FLUSH_EVERY_MS
- Fresh 100-doc import — *duration is* — 40s
- LEGACY — *reports duration of* — 40s
- BASELINE — *reports duration of* — 40s
- Fully-deduped / skipped run — *duration is* — sub-second
- server — *computes* — NFC dedup keys
- server — *matches* — existing docs
- API poll — *is also known as* — WAY1 poll
- WAY1 poll — *uses endpoint* — GET /import-jobs/{id}
- API poll — *overstates duration for* — tiny jobs
- Mongo delta — *is accurate for* — skip runs
- dedup identity — *consists of* — importZipName
- dedup identity — *consists of* — relativePath
- importZipName — *is defined as* — uploaded filename minus extension
- re-uploading same zip under same name — *causes* — skips everything
- luz-docs-import — *is related to* — ZIP entry names decode as CP437 mojibake when the UTF-8 EFS flag is unset

%% ai-graph-end %%