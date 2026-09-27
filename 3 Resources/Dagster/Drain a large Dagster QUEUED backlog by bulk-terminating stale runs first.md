---
ai_hash: 163d37e84ba976e9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities: []
source: customer360 UAT incident 2026-09-10
status: seedling
tags:
- dagster
- operations
- oom
- queue
- incident
title: Drain a large Dagster QUEUED backlog by bulk-terminating stale runs first
type: howto
---

# Drain a large Dagster QUEUED backlog by bulk-terminating stale runs first

When a `dagster-daemon` comes back after a long outage and finds a huge `QUEUED` backlog (thousands of runs), **do not just let it drain**. Bulk-**terminate** the stale/duplicate queued runs first, then let fresh jobs flow.

**Why:**
- Much of a large backlog is stale duplicates — e.g. a sensor that fired every 30s for days — not work anyone still wants run.
- With the in-process `DefaultRunLauncher`, each run executes as a subprocess **on the daemon box**. On a small box (e.g. 2 vCPU / 4 GB), churning thousands of runs back-to-back — even bounded to `max_concurrent_runs` (e.g. 2) — thrashes memory and risks an OOM storm that kills the daemon.

**How:** Dagster UI → Runs → filter `status:QUEUED` → Terminate all; or a GraphQL/CLI bulk terminate; or set `QUEUED`→`CANCELED` in the `runs` table as a last resort. Then bring the daemon up and watch memory. Seen on customer360 backend-system UAT (~3k backlog, 2026-09).

## Related
[[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

## Related

- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

%% ai-graph-start %%

**Related notes:**
- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]
- [[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]
- [[Diagnose a stalled Dagster queue daemon heartbeat vs STARTED-run age vs success rate]]
- [[Dagster runs stuck in QUEUED forever = orphaned runs leak all concurrency slots]]
- [[Dagster run_monitoring + DefaultRunLauncher does not reap STARTED zombies that predate a daemon restart]]

%% ai-graph-end %%