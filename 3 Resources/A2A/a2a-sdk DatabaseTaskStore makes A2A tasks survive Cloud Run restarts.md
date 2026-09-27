---
ai_hash: 4b7a988067d47600
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-29
entities:
- a2a-sdk
- DatabaseTaskStore
- A2A Task objects
- Cloud Run
- DefaultRequestHandler
- TaskStore
- InMemoryTaskStore
- process dict
- AsyncEngine
- a2a-sdk[postgresql]
- sqlalchemy[asyncio,postgresql-asyncpg]
- owner
- owner_resolver
- resolve_user_scope
- task id (UUID)
- build_task_store()
- GCS memory bank
- A2A protocol task bookkeeping
- Cloud SQL
- Auth-proxy unix socket
- asyncpg
source: session 2026-08-29 task-store migration
status: seedling
tags:
- a2a
- a2a-sdk
- task-store
- cloud-run
- postgres
title: a2a-sdk DatabaseTaskStore makes A2A tasks survive Cloud Run restarts
type: lesson
---

# a2a-sdk DatabaseTaskStore makes A2A tasks survive Cloud Run restarts

The a2a-sdk `DefaultRequestHandler` persists A2A `Task` objects through a `TaskStore`. The default `InMemoryTaskStore` keeps them in a process dict, so **every task is lost when the Cloud Run instance restarts, scales to zero, or gets a new revision**, and tasks are invisible to other instances (a follow-up `tasks/get` landing on a different instance returns `None`). That is only safe pinned to one always-warm instance.

`DatabaseTaskStore(engine, create_table=True)` (from `a2a-sdk[postgresql]`, which pulls `sqlalchemy[asyncio,postgresql-asyncpg]`) makes tasks durable and shareable across instances. Key behaviors:

- It takes an **already-built** `AsyncEngine` (you call `create_async_engine` yourself).
- **Lazy init**: it runs `CREATE TABLE` on first use via `_ensure_initialized`, so there is no startup `await` to wire — constructing it is synchronous and cheap.
- Tasks are keyed by `owner` (from `owner_resolver`, default `resolve_user_scope`) then task id (UUID). With a shared bearer and no per-user identity, everything lands under one owner; two agents sharing one DB can share the `tasks` table safely because ids are UUIDs.

Pattern used: a `build_task_store()` factory returns `DatabaseTaskStore` when DB env is set, else `InMemoryTaskStore`, so local dev/tests need no database. Distinct from any app-domain state (e.g. a GCS memory bank) — this store is only A2A protocol task bookkeeping.

Connect on Cloud Run via [[Cloud Run to Cloud SQL via Auth-proxy unix socket with asyncpg]].

## Related

- [[Cloud Run to Cloud SQL via Auth-proxy unix socket with asyncpg]]

%% ai-graph-start %%

**Related notes:**
- [[A2A to_a2a task_store and runner are separate persistence params]]
- [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]]
- [[Cloud Run to Cloud SQL via Auth-proxy unix socket with asyncpg]]
- [[test-agent-v2 Cloud SQL Postgres holds app + ADK-session + A2A-task tables on one engine]]
- [[One Postgres backs tasks, sessions, prompts AND pgvector in test-agent-v2]]

**Relations:**
- a2a-sdk — *includes* — DefaultRequestHandler
- DefaultRequestHandler — *uses* — TaskStore
- DefaultRequestHandler — *persists* — A2A Task objects
- TaskStore — *stores* — A2A Task objects
- InMemoryTaskStore — *implements* — TaskStore
- InMemoryTaskStore — *stores in* — process dict
- DatabaseTaskStore — *implements* — TaskStore
- DatabaseTaskStore — *enables survival of* — A2A Task objects
- DatabaseTaskStore — *enables sharing across* — Cloud Run instances
- DatabaseTaskStore — *requires* — AsyncEngine
- DatabaseTaskStore — *keys tasks by* — owner
- DatabaseTaskStore — *keys tasks by* — task id (UUID)
- a2a-sdk[postgresql] — *provides* — DatabaseTaskStore
- a2a-sdk[postgresql] — *depends on* — sqlalchemy[asyncio,postgresql-asyncpg]
- owner — *is resolved by* — owner_resolver
- owner_resolver — *defaults to* — resolve_user_scope
- build_task_store() — *returns* — DatabaseTaskStore
- build_task_store() — *returns* — InMemoryTaskStore
- DatabaseTaskStore — *is for* — A2A protocol task bookkeeping
- A2A protocol task bookkeeping — *is distinct from* — GCS memory bank
- Cloud Run — *connects to* — Cloud SQL
- Cloud Run — *connects via* — Auth-proxy unix socket
- Cloud Run — *uses* — asyncpg

%% ai-graph-end %%