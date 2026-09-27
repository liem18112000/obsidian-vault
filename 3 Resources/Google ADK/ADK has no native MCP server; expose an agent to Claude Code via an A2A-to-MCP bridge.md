---
ai_hash: 938aa8bbc7765963
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: session 2026-09-08 test-agent-v2 deploy pass
status: seedling
tags:
- google-adk
- adk
- mcp
- claude-code
- bridge
- test-agent-v2
title: ADK has no native MCP server; expose an agent to Claude Code via an A2A-to-MCP
  bridge
type: lesson
---

# ADK has no native MCP server; expose an agent to Claude Code via an A2A-to-MCP bridge

Google ADK (2.8.0) does NOT provide a way to expose one of its agents *as* an MCP server. Its MCP support is consumer-only: `McpToolset` pulls external MCP tools *into* an ADK agent. The native serving surfaces ADK ships are REST/SSE (`get_fast_api_app` / `adk api_server`), A2A (`to_a2a`), and Vertex Agent Engine — none of them MCP.

Consequence: Claude Code speaks MCP, so to let local Claude Code talk to an ADK agent you need a **custom A2A→MCP bridge** (an MCP server that forwards `tools/call` to the agent's A2A `message/send`). That bridge is the native channel; there is no framework shortcut. Vertex Agent Engine is the opposite trade-off — fully managed, but it changes the client contract and has no bridge, so it does not suit an MCP/Claude-Code consumer.

Registering the deployed bridge with Claude Code: `claude mcp add --transport http <name> <url>/mcp --header "Authorization: Bearer <token>"` (idempotent via remove-then-add). Verified while building test-agent-v2 (the Testing-Agent ADK stack), 2026-09-08.

## Related

- [[ADK InvocationContext.user_content gives the current turn's input]]

%% ai-graph-start %%

**Related notes:**
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]
- [[Claude Code speaks MCP, not A2A — an A2A agent must be bridged to be used]]
- [[A remote A2A agent needs its own connectors because MCP is client-side]]
- [[Registering a bearer-gated HTTP MCP server needs claude mcp add --header]]
- [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]]

%% ai-graph-end %%