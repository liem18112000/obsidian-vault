---
ai_hash: 7be8702829e16d21
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-07
entities: []
source: session 2026-08-07 luz_docs_import timeout work
status: seedling
tags:
- timeout
- heartbeat
- distributed-systems
- job-processing
- luz-docs
title: A per-checkpoint lastModified timestamp doubles as a liveness heartbeat for
  timeout detection
type: lesson
---

# A per-checkpoint lastModified timestamp doubles as a liveness heartbeat for timeout detection

A job/record timestamp that is rewritten on **every progress checkpoint** (e.g. a batch flush every ≤100 items or ≤1s, where the same `updateJob` write stamps `lastModified = now`) doubles as a per-second **liveness heartbeat** — you get staleness detection for free, with no external liveness probe.

**When it applies:** long-running workers that persist incremental progress.

**Detection rule:** a job is stuck/dead when `now - lastModified > threshold` **AND** its status is non-terminal. The instant the worker dies or wedges, the heartbeat freezes, so the staleness grows without bound.

**Why prefer this over a pod/liveness probe:** no `kubectl get pod` shell-out from inside the container, no RBAC, no separate healthcheck endpoint — and it also catches a *hung* worker whose pod is still 'Running' but making no progress, which a pod-status probe misses.

Discovered in luz_docs_import: `JsonStoreService.updateJob` sets `lastModifield = Instant.now()` on every call, and the import worker flushes continuously via `JobProgressWriter`, so `lastModifield` is a live heartbeat.

See [[Read-time non-persisted state repair is an anti-pattern; use a scheduled reconciler]] for how to act on the stale signal correctly.

## Related

- [[Read-time non-persisted state repair is an anti-pattern; use a scheduled reconciler]]

%% ai-graph-start %%

**Related notes:**
- [[Read-time non-persisted state repair is an anti-pattern; use a scheduled reconciler]]
- [[luz-docs-import JobProgressWriter checkpoints are for crash-durability + heartbeat, not UI progress]]
- [[luz_docs_import detects dead async-worker pods via kubectl during status polling]]
- [[Read-side fire-and-forget mutation pass the id and re-read in the async, don't mutate the object being serialized]]
- [[Durable-queue visibility timeout folds task-timeout and crash-resume into one mechanism]]

%% ai-graph-end %%