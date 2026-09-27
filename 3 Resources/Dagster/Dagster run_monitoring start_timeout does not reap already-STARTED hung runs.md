---
title: "Dagster run_monitoring start_timeout does not reap already-STARTED hung runs"
created: 2026-09-10
type: lesson
status: seedling
source: "customer360 UAT live check 2026-09-10"
tags: [dagster, run-monitoring, timeout, gotcha]
---

# Dagster run_monitoring start_timeout does not reap already-STARTED hung runs

Dagster `run_monitoring` does **not** rescue a run that is hung in `STARTED`. Its `start_timeout_seconds` only bounds the `STARTING`→`STARTED` transition (a run that never gets a live worker). Once a run reaches `STARTED`, monitoring only checks whether the run **worker is still alive** — a worker that is alive-but-stuck (deadlock, blocked I/O, orphaned after its gRPC code server died) is considered healthy and is left running. With `max_resume_run_attempts=0` it is never resumed or failed either, so it holds its `max_concurrent_runs` slot **indefinitely**.

**Implication:** `run_monitoring` guards against lost/never-started runs, NOT against hangs. To reap hangs you need a separate **run/op runtime timeout** (e.g. a `dagster/max_runtime` run tag, or an op-level timeout), which will fail the run and release its slot.

Config seen on customer360 UAT: `run_monitoring{ enabled:true, start_timeout_seconds:300, cancel_timeout_seconds:180, max_resume_run_attempts:0, poll_interval_seconds:60 }` — the MonitoringDaemon logged `Collected 2 runs for monitoring / Checking run <id>` every minute yet never reaped two 19h/25h-hung runs.

## Related
[[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]

## Related

- [[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]
