---
ai_hash: 896da1e684f84a86
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities: []
source: session 2026-08-27 — kga
status: seedling
tags:
- a2a
- a2a-sdk
- auth
- bearer
- starlette
- security
title: Declare + enforce bearer auth on an a2a-sdk 1.x server
type: howto
---

# Declare + enforce bearer auth on an a2a-sdk 1.x server

Two halves to bearer auth on an a2a-sdk 1.x agent:

**Declare it on the AgentCard (protobuf).** `security_schemes` is a `map<string, SecurityScheme>`; a bearer scheme is `SecurityScheme(http_auth_security_scheme=HTTPAuthSecurityScheme(scheme="bearer", bearer_format="opaque"))`. `security_requirements` is a repeated `SecurityRequirement`, whose `schemes` is a `map<string, StringList>` (scopes), so require it with `SecurityRequirement(schemes={"bearer": StringList(list=[])})`. All types are `a2a.types.a2a_pb2`. This is advertisement only — it tells clients to send a bearer; it does NOT enforce anything.

**Enforce it yourself** with a Starlette middleware (a2a-sdk does not gate for you). Leave discovery + probes OPEN — `str.startswith(("/.well-known/", "/healthz", "/readyz"))` — and require `Authorization: Bearer <token>` on everything else; return 401 JSON otherwise. Read the token from env PER-REQUEST (not cached at import) so it is runtime-configurable and testable via `monkeypatch.setenv`. Unset token = no enforcement (dev default).

Gotcha: the Agent Card MUST stay public even when auth is on, or clients cannot discover the agent.

Context: kga server.py + constants.py (LUZ-159671 test-agent).

## Related

- [[a2a-sdk serves the agent card at agent-card.json (new) or agent.json (old)]]

%% ai-graph-start %%

**Related notes:**
- [[a2a-sdk serves the agent card at agent-card.json (new) or agent.json (old)]]
- [[ALLOW_INSECURE=1 + blank token opens the fail-closed bearer gate]]
- [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]]
- [[Fail-open bearer auth middleware antipattern]]
- [[ADK to_a2a builds A2A routes on ASGI lifespan startup, not at construction]]

%% ai-graph-end %%