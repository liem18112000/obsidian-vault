---
ai_hash: 06528dc3b7d34913
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
source: session 2026-09-23
status: seedling
tags:
- docker-compose
- env-file
- gotcha
- mcp
- bearer
title: 'Compose: an inline comment on a BLANK env value becomes the value'
type: lesson
---

# Compose: an inline comment on a BLANK env value becomes the value

GOTCHA (bit us with a live 500): docker-compose v2 strips an inline `# comment` that follows a NON-EMPTY value (`KEY=val   # c` → `val`), but on a BLANK value the whole comment becomes the value: `GATEWAY_BEARER_TOKEN=   # blank → no token` resolved to the literal `# blank → no token` (INCLUDING the non-ASCII `→`). Downstream, the fail-closed bearer gate ran `hmac.compare_digest(auth, "Bearer <that>")` → `TypeError: comparing strings with non-ASCII characters is not supported` → every `/mcp` request 500d, so the MCP client could not connect. FIX: keep blanked env-var lines COMMENT-FREE — put the explanation on its own line above. Verify with `docker exec <svc> printenv KEY` (should print empty, not the comment). Refines [[Docker Compose v2 strips inline # comments in env_file values]]. Also: the MCP gateway endpoint is at `/mcp` (POST initialize → 200 + mcp-session-id), not the bare root; connect via `claude mcp add --transport http <name> http://localhost:8080/mcp`. See [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]].

## Related

- [[Docker Compose v2 strips inline # comments in env_file values]]

%% ai-graph-start %%

**Related notes:**
- [[Docker Compose v2 strips inline # comments in env_file values]]
- [[Registering a bearer-gated HTTP MCP server needs claude mcp add --header]]
- [[Compose command ${VAR} reads .env not env_file — use env_file + $$VAR]]
- [[Rolling a bearer-gated MCP server revision can stick Claude Code in OAuth DCR — re-register to reset]]
- [[Slow local implement_plan needs TWO timeouts raised client MCP idle + gateway A2A_CLIENT_TIMEOUT]]

%% ai-graph-end %%