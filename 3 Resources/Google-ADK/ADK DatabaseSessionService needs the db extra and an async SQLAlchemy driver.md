---
ai_hash: b08de4ab0e26e584
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-07
entities: []
source: A0 spike 2026-09-07, google-adk 2.8.0
status: seedling
tags:
- google-adk
- sessions
- sqlalchemy
- gotcha
title: ADK DatabaseSessionService needs the db extra and an async SQLAlchemy driver
type: lesson
---

# ADK DatabaseSessionService needs the db extra and an async SQLAlchemy driver

ADK's **DatabaseSessionService** (durable session/state store) is not usable out of the plain `google-adk` install. Two requirements, both discovered by running it (google-adk 2.8.0):

1. **The `[db]` extra** — `pip install "google-adk[db]"` (pulls SQLAlchemy). Plain `google-adk` raises `ImportError: The 'sqlalchemy' package is required ... install google-adk[db]`.
2. **An async SQLAlchemy driver** — its constructor calls `create_async_engine`, so the `db_url` must use an async dialect. A sync URL (`sqlite:///`, `postgresql://`) fails with *"The asyncio extension requires an async driver ... 'pysqlite' is not async."* Use:
   - **SQLite (local/spike):** `sqlite+aiosqlite:///abs/path.db` (also `pip install aiosqlite`).
   - **Postgres (prod, e.g. Cloud SQL):** `postgresql+asyncpg://…` — the same asyncpg driver a Cloud-SQL-backed stack likely already uses.

Session API is async too: `await svc.create_session(app_name=, user_id=, session_id=)` / `get_session(...)`.

Related: [[Persist ADK session state from a custom agent via Event state_delta]], [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]].

## Related

- [[Persist ADK session state from a custom agent via Event state_delta]]
- [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]]

%% ai-graph-start %%

**Related notes:**
- [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]]
- [[A2A to_a2a task_store and runner are separate persistence params]]
- [[Persist ADK session state from a custom agent via Event state_delta]]
- [[a2a-sdk DatabaseTaskStore makes A2A tasks survive Cloud Run restarts]]
- [[test-agent-v2 Cloud SQL Postgres holds app + ADK-session + A2A-task tables on one engine]]

%% ai-graph-end %%