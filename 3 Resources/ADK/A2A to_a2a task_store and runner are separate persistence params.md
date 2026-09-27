---
title: "A2A to_a2a task_store and runner are separate persistence params"
created: 2026-09-14
type: lesson
status: seedling
source: "test-agent-v2, session 2026-09-14"
tags: [adk, a2a, to_a2a, cloud-sql, task-store, session-service, gotcha]
---

# A2A to_a2a task_store and runner are separate persistence params

In Google ADK, `to_a2a(agent, *, runner=..., task_store=...)` exposes **two independent persistence knobs**, and wiring only one leaves the other in-memory:

- `runner=` carries the ADK **SessionService** (conversation/session state + events). A runner built with `DatabaseSessionService(db_engine=...)` persists session state to Postgres.
- `task_store=` is the **A2A TaskStore** (the A2A task-lifecycle state). If omitted, `to_a2a` silently creates an **in-memory** store — even when Cloud SQL is fully configured elsewhere.

Gotcha (test-agent-v2): `main.py` passed `runner=build_runner(...)` (Cloud SQL session service) but NOT `task_store=`, so the A2A task store stayed in-memory and task state was lost on redeploy/scale. Fix: build `a2a.server.tasks.DatabaseTaskStore(engine=get_engine())` from the SAME shared async engine and pass it as `task_store=`. `DatabaseTaskStore(engine, create_table=True)` lazily creates its `tasks` table (distinct from the session service tables). ADK 2.9.0 `DatabaseSessionService` and a2a `DatabaseTaskStore` both accept an `AsyncEngine`.

## Related
[[BridgeSession turn drops the answer when the A2A task completes each turn]]
