---
title: "ADK to_a2a builds A2A routes on ASGI lifespan startup, not at construction"
created: 2026-09-08
type: lesson
status: seedling
source: "test-agent-v2 gateway sweep, session 2026-09-08"
tags: [google-adk, a2a, starlette, httpx, testing, gotcha]
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
