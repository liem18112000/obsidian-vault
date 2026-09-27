---
ai_hash: 687cf3eb90d59d2a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities: []
source: session 2026-08-10 (import-job GET/DELETE usage findings)
status: seedling
tags:
- luz
- luz-docs-import
- idempotency
- design-decision
- mongodb
title: luz-docs-import JobProgressWriter checkpoints are for crash-durability + heartbeat,
  not UI progress
type: lesson
---

# luz-docs-import JobProgressWriter checkpoints are for crash-durability + heartbeat, not UI progress

In luz-docs-import, `JobProgressWriter.flushThrottled()` periodically persists the in-flight import job to Mongo (`document-import-jobs`). It looks like a progress-for-UI mechanism but is NOT: the only consumer (luz_mylife_web dashboard `checkJobStatus`) polls `status` only and reacts solely to DONE/FAILED — intermediate `successfulFiles`/`unprocessedFiles` counts are never read by any consumer.

The checkpoints actually serve two server-side purposes:
1. **Idempotency durability** — `IdempotentImportService.getImportedFilePaths` reads prior jobs` persisted `successfulFiles` + `skippedFiles` (filtered by `importZipName`) to skip already-imported files on re-import. If the pod crashes mid-import with no checkpoint, the dead job persisted nothing → a re-import dedups nothing → DUPLICATE documents.
2. **Liveness heartbeat** — `JsonStoreService.updateJob` sets `lastModifield = Instant.now()`; `failJobIfStale` marks a non-terminal job FAILED(TIMEOUT) once `lastModifield` > 3600s old. Periodic writes stop a healthy long import from being wrongly timed out.

**Decision (2026-08-10):** collapsing to a single final write was considered (since no consumer reads progress) but rejected — it would break crash-safe dedup and risk false timeouts. Instead the cadence was COARSENED: `FLUSH_EVERY_N` 100→1000, `FLUSH_EVERY_MS` 1000→10000 — keeps durability + heartbeat, ~10x fewer Mongo writes on large imports. Related: [[Luz docs-import zip flow: upload-zip returns job-id, poll GET until DONE]], [[luz-docs-import bug: rejected files not removed from unprocessedFiles]].

## Related

- [[Luz docs-import zip flow: upload-zip returns job-id]]
- [[poll GET until DONE]]
- [[luz-docs-import bug: rejected files not removed from unprocessedFiles]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import createDocument is not idempotent — server-generated id, @Retry can duplicate on lost response]]
- [[A per-checkpoint lastModified timestamp doubles as a liveness heartbeat for timeout detection]]
- [[A durable queue fixes report-write durability, not data-duplication — make the side effect idempotent]]
- [[Luz docs-import zip flow upload-zip returns job-id, poll GET until DONE]]
- [[luz_docs_import detects dead async-worker pods via kubectl during status polling]]

%% ai-graph-end %%