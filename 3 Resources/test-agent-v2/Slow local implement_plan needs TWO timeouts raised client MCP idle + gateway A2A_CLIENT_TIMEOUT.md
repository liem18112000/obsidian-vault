---
ai_hash: 6efb0f72d0c1fd65
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
source: session 2026-09-23
status: seedling
tags:
- mcp
- timeout
- a2a
- gateway
- httpx
- testing-agent
- gotcha
title: 'Slow local implement_plan needs TWO timeouts raised: client MCP idle + gateway
  A2A_CLIENT_TIMEOUT'
type: lesson
---

# Slow local implement_plan needs TWO timeouts raised: client MCP idle + gateway A2A_CLIENT_TIMEOUT

GOTCHA — a slow local implement_plan must clear TWO independent timeout layers, not one: (1) the CLIENT side, Claude Codes MCP idle timeout (default 300s; raise via per-server `"timeout"` ms in ~/.claude.json mcpServers, needs a /mcp reconnect); and (2) the SERVER side, the gateway→A2A-agent httpx READ timeout, env `A2A_CLIENT_TIMEOUT` seconds (default 600) in common/bridge/a2a_client.py. Raising only the client timeout surfaces the second wall as `httpx.ReadTimeout → RuntimeError: Cannot reach the A2A agent at http://tpd:8080/` (looks like the agent is unreachable, but it is just slow). Fix BOTH: client `"timeout": 1800000` + `A2A_CLIENT_TIMEOUT=1800` in .env.compose, then recreate the gateway (env-only, no rebuild). Root driver is claude-proxy cold-start latency × the assured loops many calls. Also: `.env.compose` holds REAL Atlassian/Bitbucket creds that are NOT in `.env.compose.example` (which has placeholders) — NEVER `cp .env.compose.example .env.compose` or you wipe the creds; append/edit the live file directly. See [[implement_plan on claude-proxy exceeds the 300s MCP idle timeout — raise per-server timeout]].

## Related

- [[implement_plan on claude-proxy exceeds the 300s MCP idle timeout — raise per-server timeout]]

%% ai-graph-start %%

**Related notes:**
- [[implement_plan on claude-proxy exceeds the 300s MCP idle timeout — raise per-server timeout]]
- [[Why local test-agent is slow claude -p ships a 17.5K agent prompt on Opus, x many serial calls]]
- [[claude-proxy login expires mid-session - implement silently degrades to heuristic stubs (score 0.00)]]
- [[Long agentic API routes need the inner run timeout below the route maxDuration]]
- [[Deployed TPD implement_plan trips Cloud Run liveness (event-loop blocked by Vertex gen)]]

%% ai-graph-end %%