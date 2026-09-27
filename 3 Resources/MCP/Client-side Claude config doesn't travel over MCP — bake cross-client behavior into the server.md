---
ai_hash: 22ea635b578dc87a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities: []
source: session 2026-08-28 (KGA dialog-persistence question)
status: seedling
tags:
- mcp
- claude
- elicitation
- server-instructions
- architecture
title: Client-side Claude config doesn't travel over MCP — bake cross-client behavior
  into the server
type: lesson
---

# Client-side Claude config doesn't travel over MCP — bake cross-client behavior into the server

Persistence mechanisms that shape a Claude client's behavior — memory files, `CLAUDE.md`, hooks, `settings.json`, an Obsidian vault — all live on the **client machine**. They do NOT travel across an MCP connection. A fresh Claude on a new machine that connects to **only** an MCP server starts with none of them, so a preference saved that way ("always ask via a Yes/No dialog") has zero effect there.

Corollary: the interactive question dialog (`AskUserQuestion`) is a **client** tool (Claude Code's UI). An MCP server cannot invoke it, and a different client (e.g. Claude Desktop) may not even have it.

To make a behavior travel with an MCP server to any client/machine, it has to be baked into the **server** itself:
- **Server `instructions`** — an MCP server advertises an `instructions` string that the connecting client loads into context. Advisory (model-followed) but travels everywhere. Easiest lever for "after tool X, suggest/ask Y".
- **MCP elicitation** — the protocol's *server-initiated* structured input request; the server asks the client to render a prompt (e.g. Yes/No). Server-driven and the closest to a hard guarantee, but depends on the client supporting elicitation and on implementing it server-side.

Neither is an absolute force (a model can ignore instructions; a client may not support elicitation), but they are the only options that live with the MCP rather than the machine. Surfaced when asked to guarantee a Yes/No dialog would appear on a fresh MCP-only client.

%% ai-graph-start %%

**Related notes:**
- [[Ship a workflow trigger from an MCP server (no client setup) via server instructions + prompts]]
- [[MCP servers load only at Claude Code startup; skills hot-reload]]
- [[Server-driven YesNo in MCP ctx.elicit + an injected Context param]]
- [[Claude Code hooks fire for any spawned claude process, not just interactive sessions]]
- [[MCP tools load at client startup registering mid-session doesn't expose them until restart]]

%% ai-graph-end %%