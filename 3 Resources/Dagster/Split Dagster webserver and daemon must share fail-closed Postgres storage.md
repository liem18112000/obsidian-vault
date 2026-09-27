---
ai_hash: f83dc1de25fbcb87
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities: []
source: customer360 UAT incident 2026-09-10
status: seedling
tags:
- dagster
- postgres
- storage
- deployment
- gotcha
title: Split Dagster webserver and daemon must share fail-closed Postgres storage
type: lesson
---

# Split Dagster webserver and daemon must share fail-closed Postgres storage

When you split `dagster dev` into separate `dagster-webserver` and `dagster-daemon` containers, they must point at the **same shared run/event/schedule storage** — normally PostgreSQL. Make that storage **mandatory (fail-closed)**: if the DB is unreachable, the container should **refuse to start**, not silently fall back to local SQLite.

**Why:** with an adaptive "fail-open" renderer, each container independently probes Postgres at boot. If one reaches it and the other (transiently) does not, the loser writes to its **own ephemeral SQLite** `DAGSTER_HOME`. The webserver then enqueues runs into one store while the daemon polls a different, empty store — so the queue "never drains" even though a daemon is running. Divergent storage is indistinguishable from "no daemon" at the symptom level.

Fail-closed keeps both processes provably on one store; a clean crash-loop is easier to diagnose than silent divergence. Seen on customer360 backend-system UAT (2026-09).

## Related
[[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

## Related

- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]

%% ai-graph-start %%

**Related notes:**
- [[Dagster QueuedRunCoordinator needs a running daemon to drain the run queue]]
- [[Scaling Dagster needs Postgres storage and a singleton daemon before adding replicas]]
- [[Dagster run-worker subprocesses don't inherit all webserver env vars]]
- [[Diagnose a stalled Dagster queue daemon heartbeat vs STARTED-run age vs success rate]]
- [[Drain a large Dagster QUEUED backlog by bulk-terminating stale runs first]]

%% ai-graph-end %%