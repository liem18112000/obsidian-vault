---
ai_hash: 462c49270a66fdea
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-29
entities: []
tags:
- claude-code
- mcp
- gotcha
title: 'MCP tools load at client startup: registering mid-session doesn''t expose
  them until restart'
type: lesson
---

# MCP tools load at client startup: registering mid-session doesn't expose them until restart

`claude mcp add <server>` writes config and `claude mcp list` may immediately show the server as "✔ Connected" — but the running Claude Code session's TOOL registry is snapshotted at startup. So the new server's tools are NOT callable in the current session; a restart is required for them to appear.

Verified: after registering test-plan-definition mid-session (list shows ✔ Connected), ToolSearch for `mcp__test-plan-definition__define_plan` returned "No matching deferred tools found". Only after a restart do the tools load.

Implication: when a flow depends on a just-added MCP server's tools, tell the user to restart Claude Code; "Connected" in `claude mcp list` is necessary but not sufficient for in-session tool availability. (Same restart also picks up new server-side code after a bridge redeploy, and new `.claude/commands/*` / project `CLAUDE.md`.)

%% ai-graph-start %%

**Related notes:**
- [[MCP servers load only at Claude Code startup; skills hot-reload]]
- [[Ship a workflow trigger from an MCP server (no client setup) via server instructions + prompts]]
- [[implement_plan on claude-proxy exceeds the 300s MCP idle timeout — raise per-server timeout]]
- [[Client-side Claude config doesn't travel over MCP — bake cross-client behavior into the server]]
- [[Registering a bearer-gated HTTP MCP server needs claude mcp add --header]]

%% ai-graph-end %%