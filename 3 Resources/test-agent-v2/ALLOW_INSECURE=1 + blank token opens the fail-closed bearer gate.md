---
ai_hash: f6968e1bfe6be0e5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
source: session 2026-09-23
status: seedling
tags:
- auth
- bearer
- security
- local-dev
- test-agent
title: ALLOW_INSECURE=1 + blank token opens the fail-closed bearer gate
type: lesson
---

# ALLOW_INSECURE=1 + blank token opens the fail-closed bearer gate

test-agent-v2 auth (common/adk/auth.py `bearer_ok`, and common/bridge/asgi.py) is FAIL-CLOSED: an expected bearer that is set → constant-time compared; a bearer that is unset/empty → 401 on every non-open path (`/livez`,`/readyz`,`/.well-known/` are open) UNLESS `ALLOW_INSECURE=1`. So to DISABLE token security for local dev: set `ALLOW_INSECURE=1` AND leave `GATEWAY_BEARER_TOKEN` + `A2A_BEARER_TOKEN` blank → the MCP gateway on :8080 and the inter-agent A2A calls need no token. A set token still wins (ALLOW_INSECURE only matters when the token is empty), so you disable per-secret by blanking it. NEVER set ALLOW_INSECURE=1 in a deployed/public env (it logs an ERROR once when firing). See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]

%% ai-graph-start %%

**Related notes:**
- [[Fail-open bearer auth middleware antipattern]]
- [[Declare + enforce bearer auth on an a2a-sdk 1.x server]]
- [[Cloud Run service-to-service with an app bearer needs the callee public (Authorization header collision)]]
- [[Bridgegateway use separate secrets for the inbound caller token and the outbound backend token]]
- [[Registering a bearer-gated HTTP MCP server needs claude mcp add --header]]

%% ai-graph-end %%