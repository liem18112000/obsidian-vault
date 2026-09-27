---
title: "ADK DatabaseSessionService needs the db extra and an async SQLAlchemy driver"
created: 2026-09-07
type: lesson
status: seedling
source: "A0 spike 2026-09-07, google-adk 2.8.0"
tags: [google-adk, sessions, sqlalchemy, gotcha]
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
