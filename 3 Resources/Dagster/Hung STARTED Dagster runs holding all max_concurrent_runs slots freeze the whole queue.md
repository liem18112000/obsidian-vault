---
title: "Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue"
created: 2026-09-10
type: lesson
status: seedling
source: "customer360 UAT live check 2026-09-10"
tags: [dagster, queue, concurrency, deadlock, incident]
---

# Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue

A Dagster `QueuedRunCoordinator` launches at most `max_concurrent_runs` runs at once. If that many runs get stuck in `STARTED` (hung, not progressing), they occupy **every** slot and the coordinator can never launch another queued run — throughput drops to **zero** while the queue grows unbounded. The daemon log shows this as a permanent `"N runs are currently in progress. Maximum is N, wont launch more."`

**Tell-tale:** many thousands `QUEUED`, but `SELECT count(*) FROM runs WHERE status=SUCCESS AND update_timestamp > now()-interval 8 hours` returns ~0, and `SELECT ... WHERE status=STARTED` shows a handful of very old runs (last `event_logs.timestamp` hours/days old).

Seen on customer360 UAT (2026-09): 2 `segmentation_job` runs hung ~19h/25h at `recompute_segments_op` step-worker spawn, holding both of `max_concurrent_runs=2`, so 4,036 runs queued and 0 drained. The proximate trigger was repeated `code_server: No heartbeat received in 20 seconds, shutting down` on a starved 2 vCPU/4 GB box killing the step worker mid-launch.

**Fix:** terminate the hung runs to free slots (immediate); add a run/step timeout so a hung step fails and releases its slot instead of holding it forever; right-size the box so the code server stops dying.

## Related
[[Dagster run_monitoring start_timeout does not reap already-STARTED hung runs]]
[[Diagnose a stalled Dagster queue: daemon heartbeat vs STARTED-run age vs success rate]]
[[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

## Related

- [[Dagster run_monitoring start_timeout does not reap already-STARTED hung runs]]
- [[Diagnose a stalled Dagster queue: daemon heartbeat vs STARTED-run age vs success rate]]
- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]
