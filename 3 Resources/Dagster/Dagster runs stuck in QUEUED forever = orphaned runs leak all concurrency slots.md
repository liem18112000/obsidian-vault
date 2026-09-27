---
ai_hash: 25a0652f49b52e5f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: LEO CDP UAT incident 2026-09-09
status: seedling
tags:
- dagster
- orchestration
- gotcha
- queue
- run-monitoring
title: Dagster runs stuck in QUEUED forever = orphaned runs leak all concurrency slots
type: lesson
---

# Dagster runs stuck in QUEUED forever = orphaned runs leak all concurrency slots

All job runs sit in `QUEUED` and none execute — for days — while the Dagster **daemon is alive and heartbeating**. The queue is deadlocked, not the daemon.

**Mechanism:** `QUEUED` only exists under the `QueuedRunCoordinator`, which launches at most `max_concurrent_runs` (default **10**). A run that starts but never reaches a terminal state — worker OOM-killed, or the subprocess never forked, so no `SUCCESS`/`FAILURE` event — stays in `STARTED`/`STARTING` and keeps counting as "in progress" forever. With `run_monitoring` **disabled**, Dagster has no way to detect the dead worker, so each such "zombie" run **leaks its concurrency slot permanently**. Once all N slots are held, the coordinator logs `"N runs are currently in progress. Maximum is N, won't launch more"` and launches nothing; sensors keep enqueuing and the queue grows without bound.

**Diagnose:** query the `dagster` Postgres `runs` table `GROUP BY status`; a deadlock shows `count(STARTED)+count(STARTING) == max_concurrent_runs`. Tell zombies apart by `running_for` age (a 5-day-old poll run is dead) and `start_time IS NULL` (never launched).

**Fix:** (a) force-fail the zombies with `DagsterInstance.report_run_failed(run, msg)` — it writes real failure events and frees the slots; (b) enable `run_monitoring` so the `MonitoringDaemon` reaps orphaned runs automatically; (c) set `max_concurrent_runs` to what the box can actually run. Aggravating factor in the LEO CDP UAT case: poll sensors enqueuing every few seconds with no dedupe on a 1 vCPU / 2 GB box, so workers OOM and re-zombie.

See also [[Dagster run_monitoring key is max_resume_run_attempts not max_resume_attempts]].

## Related

- [[Dagster run_monitoring key is max_resume_run_attempts not max_resume_attempts]]

%% ai-graph-start %%

**Related notes:**
- [[Diagnose a stalled Dagster queue daemon heartbeat vs STARTED-run age vs success rate]]
- [[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]
- [[Dagster run_monitoring + DefaultRunLauncher does not reap STARTED zombies that predate a daemon restart]]
- [[Dagster run_monitoring start_timeout does not reap already-STARTED hung runs]]
- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

%% ai-graph-end %%