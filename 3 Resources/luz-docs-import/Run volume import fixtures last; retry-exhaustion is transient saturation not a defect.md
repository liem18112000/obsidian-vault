---
ai_hash: acfc59ebeb3eb7ae
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities: []
source: dev zip-import test suite 2026-08-13
status: seedling
tags:
- luz-docs-import
- testing
- gotcha
- concurrency
- LUZ-158230
title: Run volume import fixtures last; retry-exhaustion is transient saturation not
  a defect
type: lesson
---

# Run volume import fixtures last; retry-exhaustion is transient saturation not a defect

When batch-testing the **luz-docs-import** zip-upload flow, run any high-volume fixture (e.g. a 500-document archive) **last**, and re-run any "failed" functional fixture **in isolation** before calling it a defect.

## Why
The import chain shares a downstream pipeline — **luz-antivirus** (scan) + **luz-docs** (document creation). A single large archive saturates it, and the back-pressure spills into jobs that start seconds later.

In the 2026-08-13 dev run, fixtures 09–12 reported `Failed to create document after retries` for **every** document — purely because the 500-doc fixture (08) ran immediately before them. Re-running 09–12 in isolation after the pipeline drained gave clean passes (09→2/0/3, 10→9/0/0, 11→4/0/0, 12→11/1).

## How to tell transient from real
- `Failed to create document after retries` → **transient** saturation signature (retry the fixture isolated).
- `File type is not allowed for import`, `Metadata file without matching document`, `illegal file name that breaks out of the target directory` → **real** logic/validation outcomes.

This is direct evidence for the **LUZ-158230** concurrency/flush tuning: the AV-scan + document-create path is the bottleneck under one large archive.

## Related

- [[luz-docs-import]]
- [[LUZ-158230]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)]]
- [[luz-docs-import upload-zip endpoint is the ingestion saturation point under perf load]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]
- [[luz-docs-import bug rejected files not removed from unprocessedFiles]]
- [[LUZ-158230 test approach full-chain real-deps integration with fully-materialized done]]

%% ai-graph-end %%