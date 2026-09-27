---
ai_hash: 5a13e0f49caab329
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities: []
source: session 2026-08-28
status: seedling
tags:
- mcp
- claude-code
- auth
- cloud-run
- gotcha
title: Rolling a bearer-gated MCP server revision can stick Claude Code in OAuth DCR
  — re-register to reset
type: lesson
---

# Rolling a bearer-gated MCP server revision can stick Claude Code in OAuth DCR — re-register to reset

When a remote (Streamable-HTTP) MCP server that gates requests with a static bearer token has its backing revision rolled (e.g. a Cloud Run redeploy), the live connection drops and Claude Codes client can get stuck: on reconnect it reads the servers 401 as an OAuth challenge and attempts **Dynamic Client Registration**, then reports `Failed to connect — Dynamic Client Registration rejected (HTTP 401)` — even though the configured `--header "Authorization: Bearer <token>"` is correct and a raw `curl` with that header succeeds (a real MCP `initialize` returns a proper result).

Fix: re-register the server to reset the clients cached auth/connection state:
```bash
claude mcp remove <name> -s local
claude mcp add --transport http <name> <url> --header "Authorization: Bearer $TOKEN"
```
Diagnosis tip: prove the SERVER is fine independently with an authenticated `initialize` POST before touching the client — if that returns capabilities, the problem is purely Claude Codes client state, not the server.

See [[MCP Streamable-HTTP 421 Invalid Host header behind a proxy — set TransportSecuritySettings]], [[Bridgegateway use separate secrets for the inbound caller token and the outbound backend token|Bridge-gateway use separate secrets for the inbound caller token and the outbound backend token]].

## Related

- [[MCP Streamable-HTTP 421 Invalid Host header behind a proxy — set TransportSecuritySettings]]

%% ai-graph-start %%

**Related notes:**
- [[Registering a bearer-gated HTTP MCP server needs claude mcp add --header]]
- [[claude mcp list health status can be stale; verify MCP reachability with curl]]
- [[MCP Streamable-HTTP 421 Invalid Host header behind a proxy — set TransportSecuritySettings]]
- [[Reach a private Cloud Run service as a user via gcloud run services proxy, not a minted ID token]]
- [[Prefer pasting a token minted once over scraping it from a PTY relay]]

%% ai-graph-end %%