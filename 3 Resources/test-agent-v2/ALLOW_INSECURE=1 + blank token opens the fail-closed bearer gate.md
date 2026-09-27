---
title: "ALLOW_INSECURE=1 + blank token opens the fail-closed bearer gate"
created: 2026-09-23
type: lesson
status: seedling
source: "session 2026-09-23"
tags: [auth, bearer, security, local-dev, test-agent]
---

# ALLOW_INSECURE=1 + blank token opens the fail-closed bearer gate

test-agent-v2 auth (common/adk/auth.py `bearer_ok`, and common/bridge/asgi.py) is FAIL-CLOSED: an expected bearer that is set → constant-time compared; a bearer that is unset/empty → 401 on every non-open path (`/livez`,`/readyz`,`/.well-known/` are open) UNLESS `ALLOW_INSECURE=1`. So to DISABLE token security for local dev: set `ALLOW_INSECURE=1` AND leave `GATEWAY_BEARER_TOKEN` + `A2A_BEARER_TOKEN` blank → the MCP gateway on :8080 and the inter-agent A2A calls need no token. A set token still wins (ALLOW_INSECURE only matters when the token is empty), so you disable per-secret by blanking it. NEVER set ALLOW_INSECURE=1 in a deployed/public env (it logs an ERROR once when firing). See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
