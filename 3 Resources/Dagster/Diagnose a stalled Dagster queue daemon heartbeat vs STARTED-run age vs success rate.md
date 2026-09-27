---
title: "Diagnose a stalled Dagster queue: daemon heartbeat vs STARTED-run age vs success rate"
created: 2026-09-10
type: howto
status: seedling
source: "customer360 UAT live check 2026-09-10"
tags: [dagster, diagnostics, postgres, queue, sql]
---

# Diagnose a stalled Dagster queue: daemon heartbeat vs STARTED-run age vs success rate

When a Dagster run queue is huge and "never processes", three read-only checks in the Postgres run-storage DB (`db=dagster`) tell you WHICH failure it is:

1. **Is a daemon alive and dequeuing?**
   `SELECT daemon_type, timestamp FROM daemon_heartbeats ORDER BY timestamp DESC;`
   A stale/absent `QUEUED_RUN_COORDINATOR` heartbeat => no daemon draining (e.g. webserver-only deploy).
2. **Are slots blocked by hung runs?**
   `SELECT status, count(*) FROM runs GROUP BY status;` then
   `SELECT run_id, pipeline_name, create_timestamp FROM runs WHERE status=STARTED;`
   A handful of very old `STARTED` runs = zombies holding all `max_concurrent_runs` slots.
3. **Is throughput actually zero?**
   `SELECT count(*) FROM runs WHERE status=SUCCESS AND update_timestamp > now()-interval 8 hours;`
   ~0 with a healthy heartbeat confirms slot-starvation, not a dead daemon.

Supporting: last-activity age of a stuck run via
`SELECT run_id, max(timestamp) FROM event_logs WHERE run_id=<id> GROUP BY 1;`
(and `dagster_event_type, step_key` of the last events shows WHERE it hung); break the backlog down by source with `run_tags` key `dagster/sensor_name`.

Decision: current QUEUED_RUN_COORDINATOR heartbeat + old STARTED runs + zero recent SUCCESS => hung runs deadlocking the pool (NOT a missing daemon). This distinction matters because the code/config can look correct while the live box is wedged.

## Related
[[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]
[[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]
[[A Dagster poll sensor without run_key mints a duplicate run every tick]]

## Related

- [[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]
- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]
- [[A Dagster poll sensor without run_key mints a duplicate run every tick]]
