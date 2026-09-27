---
ai_hash: eae46e9635fc48f1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- Claude Code
- MCP
- A2A
- A2A agent
- MCP tools
- A2A client
- HTTP endpoint
- Atlassian MCP
- Bridging
- Explicit instruction
- Shell shortcut
- Project skill / slash-command
- MCP proxy
- gather_knowledge (tool)
- Agent delegation protocol
- Tool calling protocol
- MCP client side
- kga test-agent
- LUZ-159671
- A remote A2A agent needs its own connectors because MCP is client-side
source: session 2026-08-27 — kga USING-FROM-CLAUDE
status: seedling
tags:
- a2a
- mcp
- claude-code
- architecture
- gotcha
title: Claude Code speaks MCP, not A2A — an A2A agent must be bridged to be used
type: lesson
---

# Claude Code speaks MCP, not A2A — an A2A agent must be bridged to be used

Claude Code natively calls **MCP** tools (Claude → tools). It has **no built-in A2A client**. So a remote **A2A** agent (agent → agent, JSON-RPC over HTTPS) is just an HTTP endpoint to Claude Code — it will NOT be auto-selected or called on its own.

Consequence/gotcha: a generic request like "gather context for TICKET-123" makes Claude use the tools it ALREADY has (e.g. a connected Atlassian MCP) and its own reasoning — NOT your deployed A2A agent. People assume "local Claude drives the A2A agent" happens automatically; it does not.

To actually route to an A2A agent from Claude Code, BRIDGE it one of four ways:
1. **Explicit instruction** — tell Claude to POST message/send to the URL with the bearer, then read results.
2. **Shell shortcut** — a function that curls message/send; run it yourself, paste output to Claude.
3. **Project skill / slash-command** — a SKILL.md that runs the curl deterministically (`/gather X`).
4. **MCP proxy** — a tiny MCP server exposing a tool (e.g. `gather_knowledge`) that calls the A2A endpoint, so it appears as a native tool. Even then, name it explicitly so Claude does not pick a competing MCP (e.g. Atlassian) instead.

A2A = protocol for agents to delegate to each other (agent cards, tasks, artifacts). MCP = protocol for a model/host to call tools+data. Different layers; Claude Code implements the MCP client side, not an A2A client.

Context: kga test-agent (LUZ-159671) — the deployed gather agent is A2A; driving it from Claude Code needs a bridge.

## Related

- [[A remote A2A agent needs its own connectors because MCP is client-side]]

%% ai-graph-start %%

**Related notes:**
- [[A remote A2A agent needs its own connectors because MCP is client-side]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]
- [[ADK has no native MCP server; expose an agent to Claude Code via an A2A-to-MCP bridge]]
- [[Atlassian MCP connector binds to one cloud site, which can differ from your REST token's site]]
- [[Slow local implement_plan needs TWO timeouts raised client MCP idle + gateway A2A_CLIENT_TIMEOUT]]

**Relations:**
- Claude Code — *uses* — MCP
- Claude Code — *calls* — MCP tools
- Claude Code — *lacks* — A2A client
- A2A agent — *is an* — HTTP endpoint
- HTTP endpoint — *for* — Claude Code
- A2A agent — *requires* — Bridging
- Bridging — *for use by* — Claude Code
- Bridging — *method* — Explicit instruction
- Bridging — *method* — Shell shortcut
- Bridging — *method* — Project skill / slash-command
- Bridging — *method* — MCP proxy
- MCP proxy — *exposes* — gather_knowledge (tool)
- gather_knowledge (tool) — *calls* — A2A agent
- A2A — *is a* — Agent delegation protocol
- MCP — *is a* — Tool calling protocol
- Claude Code — *implements* — MCP client side
- kga test-agent — *is an* — A2A agent
- kga test-agent — *is identified by* — LUZ-159671
- kga test-agent — *requires* — Bridging
- kga test-agent — *for use by* — Claude Code
- Claude Code — *uses* — Atlassian MCP
- Claude Code — *is related to* — A remote A2A agent needs its own connectors because MCP is client-side

%% ai-graph-end %%