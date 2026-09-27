---
title: "ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration"
created: 2026-09-07
type: lesson
status: seedling
source: "adk-transform export 2026-09-07"
tags: [google-adk, sessions, persistence, a2a]
---

# ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration

In Google ADK, a **DatabaseSessionService** (on a SQLAlchemy engine, e.g. Cloud SQL Postgres) stores the session, its `state`, and the event log durably. This can replace *several* bespoke persistence mechanisms at once.

**Concrete case:** an a2a-sdk agent had TWO separate durability systems — the A2A `DatabaseTaskStore` (Postgres, for the Task lifecycle) plus hand-rolled multi-turn state persisted as GCS `state.json` files with an a2a-conversation-id → domain-context-id mapping file. Both collapse into one DatabaseSessionService: `context_id → session_id`, and the loop state (pending rounds, deferred questions, counters) moves into ADK session `state`. Fewer moving parts, one place to reason about durability, and `adk web` shows the full event timeline for free.

**Rule of thumb:** if you're hand-persisting per-turn conversation state to files/blobs to survive a stateless request, ADK's SessionService is probably the built-in you're reimplementing. Keep a *domain* record store (e.g. a memory bank / knowledge graph) separate — SessionService is for conversation/session state, not your domain data model.

Related: [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]].

## Related

- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]
