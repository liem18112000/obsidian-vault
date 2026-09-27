---
ai_hash: 2b2a260acfd9127b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- A2A-to-MCP bridge
- MCP stdio server
- A2A client
- Claude
- MCP client
- A2A (Agent2Agent)
- JSON-RPC
- SSE
- Agent Card
- /.well-known/agent-card.json
- MCP
- autonomous agents
- stdio
- HTTP
- MCP tool call
- message/send
- client-side
- agent server
- MCP tool
- A2A operation
- arguments
- A2A reply
- bearer token
- HTTP call
- knowledge_gathering test-agent
- knowledge_gathering/bridge/
- a2a_client.py
- mcp_server.py
- MCPServer
- FastMCP
- mcp Python SDK 2.x
- Message
- Task envelope
- A2A input-required tasks
- taskId
- contextId
- Claude Code
- Desktop
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
- A2A-to-MCP bridge — *is_a* — MCP stdio server
- A2A-to-MCP bridge — *is_a* — A2A client
- Claude — *is_a* — MCP client
- Claude Code — *is_a* — MCP client
- Desktop — *is_a* — MCP client
- autonomous agents — *expose_themselves_over* — A2A (Agent2Agent)
- A2A (Agent2Agent) — *uses_protocol* — JSON-RPC
- A2A (Agent2Agent) — *uses_protocol* — SSE
- A2A (Agent2Agent) — *has_component* — Agent Card
- Agent Card — *has_location* — /.well-known/agent-card.json
- MCP — *does_not_interoperate_with* — A2A (Agent2Agent)
- A2A-to-MCP bridge — *enables* — Claude
- A2A-to-MCP bridge — *functions_as* — MCP stdio server
- MCP stdio server — *uses* — stdio
- A2A-to-MCP bridge — *functions_as* — A2A client
- A2A-to-MCP bridge — *forwards* — MCP tool call
- MCP tool call — *to* — message/send
- message/send — *uses_protocol* — HTTP
- A2A-to-MCP bridge — *runs_on* — client-side
- A2A-to-MCP bridge — *located_next_to* — Claude
- agent server — *needs_no* — MCP
- MCP tool — *maps_to* — A2A operation
- A2A-to-MCP bridge — *translates* — arguments
- A2A-to-MCP bridge — *translates* — A2A reply
- A2A-to-MCP bridge — *attaches* — bearer token
- bearer token — *on* — HTTP call
- A2A-to-MCP bridge — *is_implemented_in* — knowledge_gathering/bridge/
- knowledge_gathering/bridge/ — *is_part_of* — knowledge_gathering test-agent
- a2a_client.py — *implements* — A2A client
- mcp_server.py — *implements* — MCPServer
- FastMCP — *was_renamed_to* — MCPServer
- FastMCP — *renamed_in* — mcp Python SDK 2.x
- message/send — *returns* — Message
- message/send — *returns* — Task envelope
- A2A input-required tasks — *requires* — taskId
- A2A input-required tasks — *requires* — contextId

%% ai-graph-end %%