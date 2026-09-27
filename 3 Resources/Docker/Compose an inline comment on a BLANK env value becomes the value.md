---
title: "Compose: an inline comment on a BLANK env value becomes the value"
created: 2026-09-23
type: lesson
status: seedling
source: "session 2026-09-23"
tags: [docker-compose, env-file, gotcha, mcp, bearer]
---

# Compose: an inline comment on a BLANK env value becomes the value

GOTCHA (bit us with a live 500): docker-compose v2 strips an inline `# comment` that follows a NON-EMPTY value (`KEY=val   # c` → `val`), but on a BLANK value the whole comment becomes the value: `GATEWAY_BEARER_TOKEN=   # blank → no token` resolved to the literal `# blank → no token` (INCLUDING the non-ASCII `→`). Downstream, the fail-closed bearer gate ran `hmac.compare_digest(auth, "Bearer <that>")` → `TypeError: comparing strings with non-ASCII characters is not supported` → every `/mcp` request 500d, so the MCP client could not connect. FIX: keep blanked env-var lines COMMENT-FREE — put the explanation on its own line above. Verify with `docker exec <svc> printenv KEY` (should print empty, not the comment). Refines [[Docker Compose v2 strips inline # comments in env_file values]]. Also: the MCP gateway endpoint is at `/mcp` (POST initialize → 200 + mcp-session-id), not the bare root; connect via `claude mcp add --transport http <name> http://localhost:8080/mcp`. See [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]].

## Related

- [[Docker Compose v2 strips inline # comments in env_file values]]
