---
ai_hash: 91f28c3c172e2624
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-07
entities: []
source: session 2026-08-07 luz_docs_import timeout work
status: seedling
tags:
- anti-pattern
- reconciler
- idempotency
- distributed-systems
- luz-docs
title: Read-time non-persisted state repair is an anti-pattern; use a scheduled reconciler
type: lesson
---

# Read-time non-persisted state repair is an anti-pattern; use a scheduled reconciler

Repairing durable state **inside a GET/poll handler** (e.g. flipping a stuck job to FAILED/TIMEOUT when someone fetches it) is a trap with three failure modes:

1. **Poll-dependent** — the repair runs only if a client polls that exact id. A job nobody polls stays stuck forever.
2. **Not persisted** — the handler mutates the in-memory object it returns but never writes it back, so the repair is recomputed on every read and lost each time.
3. **No side-effects** — no notification / downstream event fires, because those live on the normal completion path that never ran.

**Fix:** move the transition to a **scheduled reconciler/sweeper** that runs independently of reads, **persists** the terminal state, and fires the notification **once**. Under multiple replicas, guard the flip with an **optimistic compare-and-set** (only transition if still non-terminal) so exactly one replica acts and the notification isn't double-sent.

Surfaced in luz_docs_import: the old `DocsImportService.getImportJob` did exactly this (returned a FAILED/TIMEOUT flip that was never saved and never notified). Detect staleness via [[A per-checkpoint lastModified timestamp doubles as a liveness heartbeat for timeout detection]].

## Related

- [[A per-checkpoint lastModified timestamp doubles as a liveness heartbeat for timeout detection]]

%% ai-graph-start %%

**Related notes:**
- [[Read-side fire-and-forget mutation pass the id and re-read in the async, don't mutate the object being serialized]]
- [[A per-checkpoint lastModified timestamp doubles as a liveness heartbeat for timeout detection]]
- [[Durable-queue visibility timeout folds task-timeout and crash-resume into one mechanism]]
- [[A durable queue fixes report-write durability, not data-duplication — make the side effect idempotent]]
- [[luz_docs_import detects dead async-worker pods via kubectl during status polling]]

%% ai-graph-end %%