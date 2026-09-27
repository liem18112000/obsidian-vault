---
title: "Dagster run_monitoring + DefaultRunLauncher does not reap STARTED zombies that predate a daemon restart"
created: 2026-09-09
type: gotcha
status: seedling
source: "session 2026-09-09"
tags: [dagster, run-monitoring, zombie, gotcha]
---

# Dagster run_monitoring + DefaultRunLauncher does not reap STARTED zombies that predate a daemon restart

Dagster `run_monitoring` reaps a run whose worker died only if the run launcher can health-check that worker. `DefaultRunLauncher` (subprocess launcher) cannot check a run worker that belonged to a PRIOR daemon/container instance — after a restart it has no handle on the old PID. So a run left in `STARTED` before the restart can sit there for hours holding a `max_concurrent_runs` slot, and `run_monitoring` never fails it. `start_timeout_seconds` only covers the QUEUED/STARTING->STARTED transition, not an already-STARTED run.

**Consequence:** a pre-restart zombie re-creates the concurrency deadlock even with run_monitoring enabled. **Remediation:** on deploy/restart, explicitly force-fail leftover STARTED/STARTING runs (`DagsterInstance.report_run_failed`) to free slots; do not rely on run_monitoring to clean up pre-existing zombies. For real runaway protection, set a per-run `dagster/max_runtime` tag / `max_runtime_seconds`.

## Related

- [[Dagster runs stuck in QUEUED forever = orphaned runs leak all concurrency slots]]
