---
title: "ADK has no native MCP server; expose an agent to Claude Code via an A2A-to-MCP bridge"
created: 2026-09-08
type: lesson
status: seedling
source: "session 2026-09-08 test-agent-v2 deploy pass"
tags: [google-adk, adk, mcp, claude-code, bridge, test-agent-v2]
---

# ADK has no native MCP server; expose an agent to Claude Code via an A2A-to-MCP bridge

Google ADK (2.8.0) does NOT provide a way to expose one of its agents *as* an MCP server. Its MCP support is consumer-only: `McpToolset` pulls external MCP tools *into* an ADK agent. The native serving surfaces ADK ships are REST/SSE (`get_fast_api_app` / `adk api_server`), A2A (`to_a2a`), and Vertex Agent Engine — none of them MCP.

Consequence: Claude Code speaks MCP, so to let local Claude Code talk to an ADK agent you need a **custom A2A→MCP bridge** (an MCP server that forwards `tools/call` to the agent's A2A `message/send`). That bridge is the native channel; there is no framework shortcut. Vertex Agent Engine is the opposite trade-off — fully managed, but it changes the client contract and has no bridge, so it does not suit an MCP/Claude-Code consumer.

Registering the deployed bridge with Claude Code: `claude mcp add --transport http <name> <url>/mcp --header "Authorization: Bearer <token>"` (idempotent via remove-then-add). Verified while building test-agent-v2 (the Testing-Agent ADK stack), 2026-09-08.

## Related

- [[ADK InvocationContext.user_content gives the current turn's input]]
