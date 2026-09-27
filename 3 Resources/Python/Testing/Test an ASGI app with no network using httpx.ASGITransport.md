---
ai_hash: 9eb9aa52bcbdbb77
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities: []
source: session 2026-08-27
status: seedling
tags:
- httpx
- asgi
- testing
- python
- starlette
title: Test an ASGI app with no network using httpx.ASGITransport
type: howto
---

# Test an ASGI app with no network using httpx.ASGITransport

To integration-test an ASGI app (Starlette/FastAPI, an A2A JSON-RPC server, etc.) end-to-end **without opening a socket**, wrap the app in `httpx.ASGITransport` and hand it to an `httpx.AsyncClient` as its transport:

```python
client = httpx.AsyncClient(base_url="http://app.test/", transport=httpx.ASGITransport(app=app))
```

Requests are dispatched in-process directly to the app — real routing, middleware, and handlers run, but there is no network, no free port, and no server thread. This lets a real client class (the code under test) drive a real server object in one process. It is the async analogue of Starlette's `TestClient` (which is sync and based on the same idea).

Used to test the A2A-to-MCP bridge client against the real agent app. See [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]].

## Related

- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]

%% ai-graph-start %%

**Related notes:**
- [[ADK to_a2a builds A2A routes on ASGI lifespan startup, not at construction]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]
- [[Drive the KGA A2A agent offline via Starlette TestClient for evaluation]]

%% ai-graph-end %%