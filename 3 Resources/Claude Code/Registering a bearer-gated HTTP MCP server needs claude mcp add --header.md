---
ai_hash: 1286a5b1bd38be1d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-31
entities: []
source: session 2026-08-31 mcp re-register
status: seedling
tags:
- claude-code
- mcp
- auth
- gotcha
- http
title: Registering a bearer-gated HTTP MCP server needs claude mcp add --header
type: lesson
---

# Registering a bearer-gated HTTP MCP server needs claude mcp add --header

`claude mcp add --transport http <name> <url>` registers the server, but if the endpoint is auth-gated it will show **✘ Failed to connect** in `claude mcp list` (the server responds 401 to the handshake). You must pass the credential as a header:

  claude mcp add --transport http <name> <url> --header "Authorization: Bearer <token>"

Symptom vs cause: 'Failed to connect' on an HTTP MCP server that you know is up and healthy (curl returns 401, not a timeout/refused) = missing/wrong auth header, not a networking problem. Re-add WITH the header; `claude mcp list` then shows ✔ Connected. The header (token) is stored in .claude.json under the project — fetch it from your secret store (e.g. `gcloud secrets versions access latest --secret=...`) rather than pasting it, and don't echo it. Config changes take effect in the NEXT Claude Code session. Related: the test-agent A2A→MCP bridges gate /mcp with an inbound bearer secret; see [[Co-locating a stateful MCP bridge as an agent sidecar couples their scaling]].

## Related

- [[Co-locating a stateful MCP bridge as an agent sidecar couples their scaling]]

%% ai-graph-start %%

**Related notes:**
- [[Rolling a bearer-gated MCP server revision can stick Claude Code in OAuth DCR — re-register to reset]]
- [[claude mcp list health status can be stale; verify MCP reachability with curl]]
- [[Reach a private Cloud Run service as a user via gcloud run services proxy, not a minted ID token]]
- [[ADK has no native MCP server; expose an agent to Claude Code via an A2A-to-MCP bridge]]
- [[Bridgegateway use separate secrets for the inbound caller token and the outbound backend token]]

%% ai-graph-end %%