---
ai_hash: ff5b2f4abe957139
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities: []
source: session 2026-08-27
status: seedling
tags:
- mcp
- python
- gotcha
- sdk
title: mcp Python SDK 2.x renamed FastMCP to MCPServer
type: lesson
---

# mcp Python SDK 2.x renamed FastMCP to MCPServer

In the `mcp` Python SDK **2.x**, the ergonomic server class `FastMCP` was **renamed to `MCPServer`**. Import it as `from mcp.server.mcpserver import MCPServer`. The old path `from mcp.server.fastmcp import FastMCP` now raises `ModuleNotFoundError` with a message pointing at the migration guide (or pin `mcp<2` to keep v1 code running).

The decorator API is otherwise familiar: `mcp = MCPServer("name", version=..., instructions=...)`, `@mcp.tool()` on an async function (registers it; the function stays directly callable, which is handy in tests), and `mcp.run(transport="stdio")` to serve.

Gotcha on the `Tool` objects returned by `await mcp.list_tools()`: the JSON Schema attribute is **`tool.input_schema`** (snake_case) in 2.x, not `tool.inputSchema`. The old camelCase name raises `AttributeError`.

See [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]].

## Related

- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]

%% ai-graph-start %%

**Related notes:**
- [[Expose an app as an MCP server by wrapping the same services container the webCLI use]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]
- [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]]
- [[ADK has no native MCP server; expose an agent to Claude Code via an A2A-to-MCP bridge]]
- [[MCP tools load at client startup registering mid-session doesn't expose them until restart]]

%% ai-graph-end %%