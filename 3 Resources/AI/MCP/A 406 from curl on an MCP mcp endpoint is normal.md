---
ai_hash: cf99dc3186bc0cdd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-16
entities:
- 406 Not Acceptable
- curl
- MCP endpoint
- streamable-HTTP MCP endpoint
- Accept headers
- JSON
- SSE
- server
- claude mcp get
- Excalimate
- .claudeskills
- MCP server
- port 3001
- health-check
- handshake
- broken endpoint
- MCP
- status code
source: session 2026-06-16
status: seedling
tags:
- mcp
- gotcha
- http
- debugging
title: A 406 from curl on an MCP /mcp endpoint is normal
type: lesson
---

# A 406 from curl on an MCP /mcp endpoint is normal

Hitting a streamable-HTTP MCP endpoint (e.g. `http://localhost:3001/mcp`) with a plain `curl` returns **HTTP 406 Not Acceptable** — this is expected, not an error. MCP streamable HTTP requires the client to send specific `Accept` headers (it negotiates JSON / SSE), which a bare curl does not. A 406 therefore confirms the server is up and speaking MCP; it is not a sign the endpoint is broken. To truly health-check, use `claude mcp get <name>` (which performs the proper handshake) rather than reading the curl status code.

## Related

- [[Running Excalimate locally skills in ~.claudeskills plus MCP server on port 3001]]

%% ai-graph-start %%

**Related notes:**
- [[Running Excalimate locally skills in ~.claudeskills plus MCP server on port 3001]]
- [[claude mcp list health status can be stale; verify MCP reachability with curl]]
- [[MCP servers load only at Claude Code startup; skills hot-reload]]
- [[Registering a bearer-gated HTTP MCP server needs claude mcp add --header]]
- [[A globally-bootstrapped MCP server loads into every headless claude spawn]]

**Relations:**
- curl — *receives* — 406 Not Acceptable
- 406 Not Acceptable — *occurs_on* — MCP endpoint
- 406 Not Acceptable — *is* — normal
- curl — *hits* — streamable-HTTP MCP endpoint
- streamable-HTTP MCP endpoint — *returns* — 406 Not Acceptable
- 406 Not Acceptable — *is* — expected
- 406 Not Acceptable — *is_not* — error
- streamable-HTTP MCP endpoint — *requires* — Accept headers
- streamable-HTTP MCP endpoint — *negotiates* — JSON
- streamable-HTTP MCP endpoint — *negotiates* — SSE
- curl — *does_not_send* — Accept headers
- 406 Not Acceptable — *confirms* — server
- server — *is* — up
- server — *speaks* — MCP
- 406 Not Acceptable — *is_not_sign_of* — broken endpoint
- claude mcp get — *is_used_for* — health-check
- claude mcp get — *performs* — handshake
- health-check — *should_not_rely_on* — status code
- Excalimate — *runs_in* — .claudeskills
- MCP server — *runs_on* — port 3001
- Excalimate — *uses* — MCP server

%% ai-graph-end %%