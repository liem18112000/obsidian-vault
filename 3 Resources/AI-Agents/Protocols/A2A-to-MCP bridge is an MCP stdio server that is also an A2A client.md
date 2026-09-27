---
ai_hash: 6009292d42008e46
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- A2A-to-MCP bridge
- MCP server
- stdio
- A2A client
- Claude
- Claude Code
- Desktop
- MCP client
- A2A (Agent2Agent)
- JSON-RPC
- SSE
- Agent Card
- /.well-known/agent-card.json
- HTTP
- agent server
- MCP tool
- A2A operation
- Auth
- bearer token
- knowledge_gathering test-agent
- knowledge_gathering/bridge/
- a2a_client.py
- mcp_server.py
- MCPServer
- Message
- Task envelope
- mcp Python SDK 2.x
- FastMCP
- arguments
- A2A reply
- input-required tasks
- taskId
- contextId
- autonomous agents
- message/send
source: session 2026-08-27
status: seedling
tags:
- a2a
- mcp
- bridge
- agents
- claude
title: A2A-to-MCP bridge is an MCP stdio server that is also an A2A client
type: concept
---

# A2A-to-MCP bridge is an MCP stdio server that is also an A2A client

Claude (and Claude Code / Desktop) is an **MCP** client; many autonomous agents expose themselves over **A2A** (Agent2Agent, JSON-RPC + SSE, an Agent Card at `/.well-known/agent-card.json`). The two protocols do not interoperate directly. The bridge that lets Claude drive an A2A agent is a single process that is **both**: an MCP server on stdio (the face Claude launches and calls tools on) and an A2A client (which forwards each MCP tool call to the agent's `message/send` over HTTP).

Key consequences:
- The bridge runs **client-side**, next to Claude — the agent server is unchanged and needs no MCP.
- Each MCP tool maps to one A2A operation; the bridge translates arguments in and the A2A reply out.
- Auth passes through: the bridge attaches the agent's bearer token on each HTTP call.

Implemented in the `knowledge_gathering` test-agent as `knowledge_gathering/bridge/` (a2a_client.py = A2A half, no mcp import; mcp_server.py = MCPServer tools). See [[mcp Python SDK 2.x renamed FastMCP to MCPServer]], [[A2A messagesend returns either a Message or a Task envelope|A2A message/send returns either a Message or a Task envelope]], [[A2A input-required tasks must be answered on the same taskId and contextId]].

## Related

- [[mcp Python SDK 2.x renamed FastMCP to MCPServer]]
- [[A2A messagesend returns either a Message or a Task envelope|A2A message/send returns either a Message or a Task envelope]]
- [[A2A input-required tasks must be answered on the same taskId and contextId]]

%% ai-graph-start %%

**Related notes:**
- [[Claude Code speaks MCP, not A2A — an A2A agent must be bridged to be used]]
- [[ADK has no native MCP server; expose an agent to Claude Code via an A2A-to-MCP bridge]]
- [[A remote A2A agent needs its own connectors because MCP is client-side]]
- [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]]
- [[mcp Python SDK 2.x renamed FastMCP to MCPServer]]

**Relations:**
- A2A-to-MCP bridge — *is_a* — MCP server
- A2A-to-MCP bridge — *operates_on* — stdio
- A2A-to-MCP bridge — *is_a* — A2A client
- Claude — *is_a* — MCP client
- Claude Code — *is_a* — MCP client
- Desktop — *is_a* — MCP client
- autonomous agents — *expose_themselves_over* — A2A (Agent2Agent)
- A2A (Agent2Agent) — *is_based_on* — JSON-RPC
- A2A (Agent2Agent) — *is_based_on* — SSE
- A2A (Agent2Agent) — *defines* — Agent Card
- Agent Card — *located_at* — /.well-known/agent-card.json
- MCP client — *does_not_interoperate_with* — A2A (Agent2Agent)
- A2A (Agent2Agent) — *does_not_interoperate_with* — MCP client
- A2A-to-MCP bridge — *enables* — Claude
- Claude — *to_drive* — A2A (Agent2Agent)
- A2A client — *forwards_call_to* — message/send
- A2A client — *uses_protocol* — HTTP
- A2A-to-MCP bridge — *runs_on* — client-side
- A2A-to-MCP bridge — *runs_next_to* — Claude
- agent server — *is* — unchanged
- agent server — *requires_no* — MCP
- MCP tool — *maps_to* — A2A operation
- A2A-to-MCP bridge — *translates* — arguments
- A2A-to-MCP bridge — *translates* — A2A reply
- Auth — *passes_through* — A2A-to-MCP bridge
- A2A-to-MCP bridge — *attaches* — bearer token
- bearer token — *on* — HTTP
- A2A-to-MCP bridge — *implemented_in* — knowledge_gathering test-agent
- knowledge_gathering/bridge/ — *is_part_of* — knowledge_gathering test-agent
- a2a_client.py — *is_a_component_of* — knowledge_gathering/bridge/
- mcp_server.py — *is_a_component_of* — knowledge_gathering/bridge/
- a2a_client.py — *implements* — A2A half
- mcp_server.py — *implements* — MCPServer tools
- mcp Python SDK 2.x — *renamed* — FastMCP
- FastMCP — *to* — MCPServer
- message/send — *returns* — Message
- message/send — *returns* — Task envelope
- input-required tasks — *requires_same* — taskId
- input-required tasks — *requires_same* — contextId

%% ai-graph-end %%