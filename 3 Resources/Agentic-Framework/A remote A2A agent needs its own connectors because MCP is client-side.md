---
ai_hash: b963978dc143e844
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- Remote A2A agent
- Connectors
- MCP
- GCP Cloud Run
- Local Claude
- Agent2Agent (A2A) protocol
- External system
- Jira
- Confluence
- API connector
- Claude client
- Developer's machine
- MCP servers
- Atlassian
- REST client
- Secret Manager
- Governance rule
- Short-term memory
- Long-term memory
- Task history
- '`contextId`'
- GCS bucket
- Markdown notes
- Link-graph index
- Run-logs
- LUZ-159671 Testing-Agent
- Knowledge-Gathering loop
- Bounded frontier crawl
- Verify edge
- GET endpoints
- secret
- write scopes
- messages/tasks
- explicit allowlist
source: session 2026-08-27 — test-agent proposal
status: seedling
tags:
- a2a
- mcp
- architecture
- agentic
title: A remote A2A agent needs its own connectors because MCP is client-side
type: lesson
---

# A remote A2A agent needs its own connectors because MCP is client-side

When a **remote agent runs unattended on a server (e.g. GCP Cloud Run)** and is orchestrated by **local Claude over the Agent2Agent (A2A) protocol**, the remote agent must ship its **own minimal API connector** to any external system (Jira, Confluence, …) — it cannot borrow Claude's MCP servers.

**Why:** MCP servers are **client-side** — they live next to the Claude client on the developer's machine. An A2A *remote* agent is a different process on a different host with no access to them. So "the agent can already reach Atlassian via MCP" is false for the remote half. Give the remote agent a narrow, declared, **read-only** REST client instead (a handful of GET endpoints, one secret from Secret Manager, zero write scopes) — which also satisfies the "connectors declared as an explicit allowlist" governance rule.

**Corollary — memory split under A2A:**
- **Short-term memory is free** in A2A: task history + `contextId` group related messages/tasks for you.
- **Long-term memory is yours to build** — it does not come with the protocol. Here: a GCS bucket of markdown notes + a link-graph index + run-logs, deliberately *curated facts, not a transcript*.

Context: LUZ-159671 Testing-Agent first slice.

## Related

- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]

%% ai-graph-start %%

**Related notes:**
- [[Claude Code speaks MCP, not A2A — an A2A agent must be bridged to be used]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]
- [[Atlassian MCP connector binds to one cloud site, which can differ from your REST token's site]]
- [[ADK has no native MCP server; expose an agent to Claude Code via an A2A-to-MCP bridge]]
- [[Run test-agent-v2 locally with docker-compose (no GCP)]]

**Relations:**
- Remote A2A agent — *requires* — Connectors
- MCP — *is* — client-side
- Remote A2A agent — *runs on* — GCP Cloud Run
- Remote A2A agent — *orchestrated by* — Local Claude
- Remote A2A agent — *uses* — Agent2Agent (A2A) protocol
- Remote A2A agent — *ships* — API connector
- API connector — *connects to* — External system
- External system — *includes* — Jira
- External system — *includes* — Confluence
- Remote A2A agent — *cannot borrow* — MCP servers
- MCP servers — *are* — client-side
- MCP servers — *reside with* — Claude client
- Claude client — *is on* — Developer's machine
- Remote A2A agent — *is a* — different process
- Remote A2A agent — *is on a* — different host
- Remote A2A agent — *lacks access to* — MCP servers
- MCP — *provides access to* — Atlassian
- Remote A2A agent — *should use* — REST client
- REST client — *is* — read-only
- REST client — *contains* — GET endpoints
- REST client — *uses* — secret
- secret — *from* — Secret Manager
- REST client — *has* — zero write scopes
- Connectors — *are declared as* — explicit allowlist
- explicit allowlist — *is a* — Governance rule
- Short-term memory — *is free in* — Agent2Agent (A2A) protocol
- Task history — *groups* — messages/tasks
- `contextId` — *groups* — messages/tasks
- Long-term memory — *is* — user-built
- Long-term memory — *not included with* — Agent2Agent (A2A) protocol
- Long-term memory — *example* — GCS bucket
- GCS bucket — *stores* — Markdown notes
- Long-term memory — *example* — Link-graph index
- Long-term memory — *example* — Run-logs
- LUZ-159671 Testing-Agent — *is* — first slice
- Knowledge-Gathering loop — *is a* — Bounded frontier crawl
- Knowledge-Gathering loop — *has a* — Verify edge

%% ai-graph-end %%