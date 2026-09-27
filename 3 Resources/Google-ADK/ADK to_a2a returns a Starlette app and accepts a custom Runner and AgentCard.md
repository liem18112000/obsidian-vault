---
title: "ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard"
created: 2026-09-07
type: lesson
status: seedling
source: "A.0 build 2026-09-07, google-adk 2.8.0"
tags: [google-adk, a2a, serving, starlette]
---

# ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard

ADK's `to_a2a(agent, *, host, port, protocol, rpc_path='', agent_card=None, task_store=None, runner=None, lifespan=None, ...)` (from `google.adk.a2a.utils.agent_to_a2a`) converts a BaseAgent/Workflow into an **A2A Starlette application** and auto-generates the well-known AgentCard from the agent. Two params make it the clean replacement for a hand-rolled A2A server:

- **`runner=`** — pass your own `Runner` (with a durable `DatabaseSessionService`, artifact service, and plugins) instead of letting to_a2a build a default in-memory one.
- **`agent_card=`** — pass a pre-built AgentCard (or path) for exact skill parity when a client depends on specific skill ids.

Because it returns a **Starlette** instance, you can still `app.add_middleware(...)` (e.g. a bearer gate) and `app.router.routes.extend([...])` (e.g. /livez /readyz health routes) before serving with uvicorn — so migrating an existing a2a-sdk server to ADK keeps the same auth + health surface. Verified on google-adk 2.8.0. Related: [[ADK DatabaseSessionService needs the db extra and an async SQLAlchemy driver]], [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]].

## Related

- [[ADK DatabaseSessionService needs the db extra and an async SQLAlchemy driver]]
- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]
