---
ai_hash: c2d66ddc3bd044bf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: test-agent-v2 gateway sweep, session 2026-09-08
status: seedling
tags:
- google-adk
- a2a
- starlette
- httpx
- testing
- gotcha
title: ADK to_a2a builds A2A routes on ASGI lifespan startup, not at construction
type: lesson
---

# ADK to_a2a builds A2A routes on ASGI lifespan startup, not at construction

Google ADK's `to_a2a(root, runner=...)` returns a Starlette app whose A2A endpoints — `POST /` (JSON-RPC `message/send`) and `GET /.well-known/agent-card.json` — do **not** exist at construction time. `app.routes` is empty until the ASGI **lifespan startup** runs, because the agent card is built asynchronously inside a `_combined_lifespan` handler.

**The gotcha:** `httpx.ASGITransport(app=app)` does NOT fire ASGI lifespan events. So an in-process test that points a client at a freshly-built `to_a2a` app gets `404 Not Found` for `POST /` — the routes were never attached.

**The fix:** drive the lifespan yourself and keep it open for the duration of the calls:

```python
app = to_a2a(root, runner=Runner(app_name=root.name, agent=root, session_service=InMemorySessionService()))
async with app.router.lifespan_context(app):
    client = A2ABridgeClient(base_url="http://x/", transport=httpx.ASGITransport(app=app))
    ... # POST / now routes correctly
```

Routes persist while the context is open; `__aexit__` tears down. In production this is a non-issue — uvicorn fires lifespan on boot. It only bites in-process ASGITransport tests. (Starlette `TestClient` runs lifespan too, but it is sync.)

## Related

- [[ADK to_a2a for A2A serving]]

%% ai-graph-start %%

**Related notes:**
- [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]]
- [[ADK to_a2a auto-card is generic; pass agent_card= to keep a rich AgentCard]]
- [[a2a-sdk serves the agent card at agent-card.json (new) or agent.json (old)]]
- [[How ADK agents are deployed adk deploy cloud_run agent_engine gke + get_fast_api_app]]
- [[Test an ASGI app with no network using httpx.ASGITransport]]

%% ai-graph-end %%