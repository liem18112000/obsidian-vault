---
ai_hash: 9282b457f06e4992
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-07
entities: []
source: A.0 build 2026-09-07, google-adk 2.8.0
status: seedling
tags:
- google-adk
- a2a
- serving
- starlette
title: ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard
type: lesson
---

# ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard

ADK's `to_a2a(agent, *, host, port, protocol, rpc_path='', agent_card=None, task_store=None, runner=None, lifespan=None, ...)` (from `google.adk.a2a.utils.agent_to_a2a`) converts a BaseAgent/Workflow into an **A2A Starlette application** and auto-generates the well-known AgentCard from the agent. Two params make it the clean replacement for a hand-rolled A2A server:

- **`runner=`** — pass your own `Runner` (with a durable `DatabaseSessionService`, artifact service, and plugins) instead of letting to_a2a build a default in-memory one.
- **`agent_card=`** — pass a pre-built AgentCard (or path) for exact skill parity when a client depends on specific skill ids.

Because it returns a **Starlette** instance, you can still `app.add_middleware(...)` (e.g. a bearer gate) and `app.router.routes.extend([...])` (e.g. /livez /readyz health routes) before serving with uvicorn — so migrating an existing a2a-sdk server to ADK keeps the same auth + health surface. Verified on google-adk 2.8.0. Related: [[ADK DatabaseSessionService needs the db extra and an async SQLAlchemy driver]], [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]].

## Related

- [[ADK DatabaseSessionService needs the db extra and an async SQLAlchemy driver]]
- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]

%% ai-graph-start %%

**Related notes:**
- [[ADK to_a2a builds A2A routes on ASGI lifespan startup, not at construction]]
- [[ADK to_a2a auto-card is generic; pass agent_card= to keep a rich AgentCard]]
- [[How ADK agents are deployed adk deploy cloud_run agent_engine gke + get_fast_api_app]]
- [[a2a-sdk serves the agent card at agent-card.json (new) or agent.json (old)]]
- [[A2A to_a2a task_store and runner are separate persistence params]]

%% ai-graph-end %%