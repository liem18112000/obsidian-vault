---
title: "Dagster QueuedRunCoordinator needs a running daemon to drain the run queue"
created: 2026-09-10
type: lesson
status: seedling
source: "customer360 UAT incident 2026-09-10"
tags: [dagster, orchestration, gotcha, queue]
---

# Dagster QueuedRunCoordinator needs a running daemon to drain the run queue

With a `QueuedRunCoordinator`, submitted Dagster runs land in `QUEUED` and are dequeued/launched **only** by the `dagster-daemon` process. The `dagster-webserver` (UI/GraphQL) runs **no** daemon on its own, so a webserver-only deployment accepts jobs and enqueues them forever while nothing ever launches them — the classic symptom being thousands of runs stuck `QUEUED` and "never processing".

**When it bites:** splitting `dagster dev` (which bundles webserver + daemon) into separate containers/processes and forgetting the daemon; or a daemon that crash-loops/OOMs.

**Diagnose:**
- Confirm a `dagster-daemon run -w workspace.yaml` process/container exists and is Up — and **exactly one** (there is no leader election; two daemons launch duplicate schedule/sensor runs).
- Tail the daemon logs for `QueuedRunCoordinatorDaemon` activity + heartbeat.
- `SELECT status, count(*) FROM runs GROUP BY status;` in the Dagster Postgres DB to see `QUEUED` vs `STARTED`/`FAILURE`.

Seen on customer360 backend-system UAT (2026-09): the single-container deploy ran the webserver only, so ~3k runs piled up in `QUEUED`.

## Related
[[Split Dagster webserver and daemon must share fail-closed Postgres storage]]
[[Drain a large Dagster QUEUED backlog by bulk-terminating stale runs first]]

## Related

- [[Split Dagster webserver and daemon must share fail-closed Postgres storage]]
- [[Drain a large Dagster QUEUED backlog by bulk-terminating stale runs first]]
