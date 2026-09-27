---
ai_hash: 2f4b49b9ab1e2d09
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-18
entities:
- test-agent-v2
- Cloud SQL Postgres
- persistence database
- async SQLAlchemy engine
- common.db.get_engine
- pgvector
- build_session_service()
- build_task_store()
- app-owned tables
- memory_node
- memory_edge
- prompt_template
- prompt_version
- ADK DatabaseSessionService tables
- sessions
- events
- app_states
- user_states
- adk_internal_metadata
- A2A DatabaseTaskStore tables
- tasks
- context_id
- GCS
- code graphs
- harvested lessons
- EER diagram
- test-agent-v2/docs/persistence-db-eer.excalidraw
- persistence-db-eer.png
- live version
- HNSW index
- GIN index
source: session 2026-09-18 building persistence-db-eer
status: seedling
tags:
- test-agent-v2
- cloud-sql
- postgres
- pgvector
- adk
- a2a
- schema
title: test-agent-v2 Cloud SQL Postgres holds app + ADK-session + A2A-task tables
  on one engine
type: reference
---

# test-agent-v2 Cloud SQL Postgres holds app + ADK-session + A2A-task tables on one engine

The test-agent-v2 persistence database is a single Cloud SQL Postgres instance reached through one shared async SQLAlchemy engine (`common.db.get_engine`, pgvector extension enabled). It holds three groups of tables, and both `build_session_service()` and `build_task_store()` dial the *same* engine/pool.

**App-owned (we write the DDL):**
- `memory_node` — pgvector recall tier. `id` PK; `embedding vector(768)`; `tsv` is a GENERATED tsvector column; HNSW index on embedding + GIN on tsv; `meta jsonb`; carries `run_id`, `context_id`, `scope`, `status`, `confidence`.
- `memory_edge` — directed memory graph. PK `(source_id, target)`. Both columns reference `memory_node.id` but are plain `text` — an **app-level** reference, no DB foreign key.
- `prompt_template` — PK `key`; `current_version` points at the live version.
- `prompt_version` — PK `(key, version)`; append-only audit log; `key` FK -> `prompt_template`.

**ADK `DatabaseSessionService` (auto-created, we do not own the DDL):** `sessions` (PK `app_name,user_id,id`), `events` (PK `id,app_name,user_id,session_id`; FK -> `sessions`), `app_states` (PK `app_name`), `user_states` (PK `app_name,user_id`), `adk_internal_metadata` (PK `key`, schema-version marker).

**A2A `DatabaseTaskStore` (auto-created):** `tasks` — PK `id text(36)`; `context_id`; `status`/`artifacts`/`history`/`metadata` as `json`.

`context_id` is the app-level thread tying A2A tasks <-> ADK sessions <-> memory nodes into one pipeline context (no DB FK across the groups).

**Not in this DB:** code graphs and harvested lessons persist in **GCS** (object store), not Cloud SQL.

EER diagram: `test-agent-v2/docs/persistence-db-eer.excalidraw` (+ `.png`).

## Related

- [[test-agent-v2]]

%% ai-graph-start %%

**Related notes:**
- [[One Postgres backs tasks, sessions, prompts AND pgvector in test-agent-v2]]
- [[A2A to_a2a task_store and runner are separate persistence params]]
- [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]]
- [[a2a-sdk DatabaseTaskStore makes A2A tasks survive Cloud Run restarts]]
- [[test-agent-v2 cloud resource and credential map (klara-nonprod)]]

**Relations:**
- test-agent-v2 — *uses* — Cloud SQL Postgres
- Cloud SQL Postgres — *is* — persistence database
- persistence database — *is* — single instance
- persistence database — *reached via* — async SQLAlchemy engine
- async SQLAlchemy engine — *is* — common.db.get_engine
- async SQLAlchemy engine — *has extension* — pgvector
- build_session_service() — *dials* — async SQLAlchemy engine
- build_task_store() — *dials* — async SQLAlchemy engine
- persistence database — *holds* — app-owned tables
- persistence database — *holds* — ADK DatabaseSessionService tables
- persistence database — *holds* — A2A DatabaseTaskStore tables
- app-owned tables — *includes* — memory_node
- app-owned tables — *includes* — memory_edge
- app-owned tables — *includes* — prompt_template
- app-owned tables — *includes* — prompt_version
- memory_node — *is* — pgvector recall tier
- memory_node — *has primary key* — id
- memory_node — *has column* — embedding
- memory_node — *has column* — tsv
- memory_node — *has index* — HNSW index on embedding
- memory_node — *has index* — GIN index on tsv
- memory_node — *has column* — meta
- memory_node — *carries* — run_id
- memory_node — *carries* — context_id
- memory_node — *carries* — scope
- memory_node — *carries* — status
- memory_node — *carries* — confidence
- memory_edge — *is* — directed memory graph
- memory_edge — *has primary key* — (source_id, target)
- memory_edge — *references* — memory_node
- prompt_template — *has primary key* — key
- prompt_template — *has column* — current_version
- current_version — *points to* — live version
- prompt_version — *has primary key* — (key, version)
- prompt_version — *has foreign key to* — prompt_template
- ADK DatabaseSessionService tables — *includes* — sessions
- ADK DatabaseSessionService tables — *includes* — events
- ADK DatabaseSessionService tables — *includes* — app_states
- ADK DatabaseSessionService tables — *includes* — user_states
- ADK DatabaseSessionService tables — *includes* — adk_internal_metadata
- sessions — *has primary key* — (app_name, user_id, id)
- events — *has primary key* — (id, app_name, user_id, session_id)
- events — *has foreign key to* — sessions
- app_states — *has primary key* — app_name
- user_states — *has primary key* — (app_name, user_id)
- adk_internal_metadata — *has primary key* — key
- A2A DatabaseTaskStore tables — *includes* — tasks
- tasks — *has primary key* — id
- tasks — *has column* — context_id
- tasks — *has column* — status
- tasks — *has column* — artifacts
- tasks — *has column* — history
- tasks — *has column* — metadata
- context_id — *links* — tasks
- context_id — *links* — sessions
- context_id — *links* — memory_node
- code graphs — *persist in* — GCS
- harvested lessons — *persist in* — GCS
- test-agent-v2 — *has* — EER diagram
- EER diagram — *is located at* — test-agent-v2/docs/persistence-db-eer.excalidraw
- test-agent-v2/docs/persistence-db-eer.excalidraw — *has version* — persistence-db-eer.png

%% ai-graph-end %%