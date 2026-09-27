---
ai_hash: 7afe663097f81b24
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-07
entities: []
source: adk-transform export 2026-09-07
status: seedling
tags:
- google-adk
- sessions
- persistence
- a2a
title: ADK DatabaseSessionService can subsume a separate A2A task store and state-file
  rehydration
type: lesson
---

# ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration

In Google ADK, a **DatabaseSessionService** (on a SQLAlchemy engine, e.g. Cloud SQL Postgres) stores the session, its `state`, and the event log durably. This can replace *several* bespoke persistence mechanisms at once.

**Concrete case:** an a2a-sdk agent had TWO separate durability systems — the A2A `DatabaseTaskStore` (Postgres, for the Task lifecycle) plus hand-rolled multi-turn state persisted as GCS `state.json` files with an a2a-conversation-id → domain-context-id mapping file. Both collapse into one DatabaseSessionService: `context_id → session_id`, and the loop state (pending rounds, deferred questions, counters) moves into ADK session `state`. Fewer moving parts, one place to reason about durability, and `adk web` shows the full event timeline for free.

**Rule of thumb:** if you're hand-persisting per-turn conversation state to files/blobs to survive a stateless request, ADK's SessionService is probably the built-in you're reimplementing. Keep a *domain* record store (e.g. a memory bank / knowledge graph) separate — SessionService is for conversation/session state, not your domain data model.

Related: [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]].

## Related

- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]

%% ai-graph-start %%

**Related notes:**
- [[A2A to_a2a task_store and runner are separate persistence params]]
- [[Persist ADK session state from a custom agent via Event state_delta]]
- [[ADK DatabaseSessionService needs the db extra and an async SQLAlchemy driver]]
- [[a2a-sdk DatabaseTaskStore makes A2A tasks survive Cloud Run restarts]]
- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]

%% ai-graph-end %%