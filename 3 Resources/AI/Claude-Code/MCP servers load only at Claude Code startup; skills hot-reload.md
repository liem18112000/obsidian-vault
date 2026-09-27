---
ai_hash: 040b987d9b936745
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-16
entities:
- MCP servers
- Claude Code
- skills
- skills folders
- SKILL.md
- MCP tools
- claude mcp add
- hot-reload
- startup
- claude mcp list
- claude mcp get <name>
- ~.claude
source: session 2026-06-16
status: seedling
tags:
- claude-code
- mcp
- skills
- gotcha
title: MCP servers load only at Claude Code startup; skills hot-reload
type: lesson
---

# MCP servers load only at Claude Code startup; skills hot-reload

Claude Code discovers **skills** by scanning the skills folders continuously, so a newly added or edited `SKILL.md` becomes available within the same running session (hot-reload). **MCP servers** are different: they are loaded once at startup, so adding one with `claude mcp add` does NOT expose its tools to the session you ran the command in — you must restart Claude Code to pick up the new MCP tools.

So after wiring a new MCP server, expect: skills usable immediately, MCP tools only after restart. `claude mcp list` / `claude mcp get <name>` reporting "Connected" only confirms the server is reachable, not that the current session has loaded its tools.

Related: [[Claude Code holds an open handle on every skills folder under ~.claude]].

## Related

- [[Claude Code holds an open handle on every skills folder under ~.claude]]

%% ai-graph-start %%

**Related notes:**
- [[MCP tools load at client startup registering mid-session doesn't expose them until restart]]
- [[Claude Code holds an open handle on every skills folder under ~.claude]]
- [[Client-side Claude config doesn't travel over MCP — bake cross-client behavior into the server]]
- [[A 406 from curl on an MCP mcp endpoint is normal]]
- [[Claude Code speaks MCP, not A2A — an A2A agent must be bridged to be used]]

**Relations:**
- MCP servers — *load during* — startup
- skills — *support* — hot-reload
- Claude Code — *discovers* — skills
- Claude Code — *scans* — skills folders
- SKILL.md — *defines* — skills
- claude mcp add — *adds* — MCP servers
- MCP servers — *provide* — MCP tools
- MCP tools — *require restart of* — Claude Code
- skills — *become available* — immediately
- claude mcp list — *reports status of* — MCP servers
- claude mcp get <name> — *reports status of* — MCP servers
- Claude Code — *holds open handle on* — skills folders
- skills folders — *are located under* — ~.claude

%% ai-graph-end %%