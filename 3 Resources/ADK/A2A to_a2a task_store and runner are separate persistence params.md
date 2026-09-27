---
ai_hash: bc42e0f422b62aed
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities:
- A2A
- to_a2a
- task_store
- runner
- Google ADK
- persistence knobs
- SessionService
- conversation state
- session state
- events
- DatabaseSessionService
- Postgres
- A2A TaskStore
- A2A task-lifecycle state
- in-memory store
- Cloud SQL
- test-agent-v2
- main.py
- a2a.server.tasks.DatabaseTaskStore
- async engine
- tasks table
- ADK 2.9.0
- AsyncEngine
- BridgeSession
- session service tables
- task state
source: test-agent-v2, session 2026-09-14
status: seedling
tags:
- adk
- a2a
- to_a2a
- cloud-sql
- task-store
- session-service
- gotcha
title: A2A to_a2a task_store and runner are separate persistence params
type: lesson
---

# A2A to_a2a task_store and runner are separate persistence params

In Google ADK, `to_a2a(agent, *, runner=..., task_store=...)` exposes **two independent persistence knobs**, and wiring only one leaves the other in-memory:

- `runner=` carries the ADK **SessionService** (conversation/session state + events). A runner built with `DatabaseSessionService(db_engine=...)` persists session state to Postgres.
- `task_store=` is the **A2A TaskStore** (the A2A task-lifecycle state). If omitted, `to_a2a` silently creates an **in-memory** store — even when Cloud SQL is fully configured elsewhere.

Gotcha (test-agent-v2): `main.py` passed `runner=build_runner(...)` (Cloud SQL session service) but NOT `task_store=`, so the A2A task store stayed in-memory and task state was lost on redeploy/scale. Fix: build `a2a.server.tasks.DatabaseTaskStore(engine=get_engine())` from the SAME shared async engine and pass it as `task_store=`. `DatabaseTaskStore(engine, create_table=True)` lazily creates its `tasks` table (distinct from the session service tables). ADK 2.9.0 `DatabaseSessionService` and a2a `DatabaseTaskStore` both accept an `AsyncEngine`.

## Related
[[BridgeSession turn drops the answer when the A2A task completes each turn]]

%% ai-graph-start %%

**Related notes:**
- [[a2a-sdk DatabaseTaskStore makes A2A tasks survive Cloud Run restarts]]
- [[BridgeSession turn drops the answer when the A2A task completes each turn]]
- [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]]
- [[test-agent-v2 Cloud SQL Postgres holds app + ADK-session + A2A-task tables on one engine]]
- [[One Postgres backs tasks, sessions, prompts AND pgvector in test-agent-v2]]

**Relations:**
- to_a2a — *exposes* — persistence knobs
- persistence knobs — *include* — runner
- persistence knobs — *include* — task_store
- runner — *carries* — SessionService
- SessionService — *manages* — conversation state
- SessionService — *manages* — session state
- SessionService — *manages* — events
- DatabaseSessionService — *persists* — session state
- session state — *persists to* — Postgres
- task_store — *is* — A2A TaskStore
- A2A TaskStore — *manages* — A2A task-lifecycle state
- to_a2a — *creates* — in-memory store
- in-memory store — *is created if* — task_store is omitted
- test-agent-v2 — *is a* — Gotcha
- main.py — *passed* — runner
- main.py — *did not pass* — task_store
- A2A task store — *remained* — in-memory
- task state — *was* — lost
- a2a.server.tasks.DatabaseTaskStore — *is a* — Fix
- a2a.server.tasks.DatabaseTaskStore — *accepts* — engine
- engine — *is a* — shared async engine
- a2a.server.tasks.DatabaseTaskStore — *creates* — tasks table
- tasks table — *is distinct from* — session service tables
- ADK 2.9.0 — *DatabaseSessionService accepts* — AsyncEngine
- a2a.server.tasks.DatabaseTaskStore — *accepts* — AsyncEngine
- BridgeSession — *is related to* — A2A

%% ai-graph-end %%