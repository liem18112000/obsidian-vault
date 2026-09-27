---
ai_hash: 0066ca02d8bc2e38
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-31
entities:
- A2A AgentSkill
- advertisement metadata
- executor routing
- a2a-sdk
- Agent Card
- clients
- capabilities
- AgentExecutor
- SDK
- skill id
- handler
- Dispatch
- execute(context, event_queue)
- message
- A2A app
- DefaultRequestHandler
- routing table
- promise
- implementation
- MCP tools
- tool-name
- handler function
- test-agent repo
- search-memory
- get-note
- bridge/MCP side
- menu
- kitchen
- name→function surface
- create_agent_card_routes
- create_jsonrpc_routes
- test-agent two A2A agents share a skeleton but diverge in domain engines
source: session 2026-08-31 test-agent
status: seedling
tags:
- a2a
- agent-card
- executor
- mcp
- gotcha
- architecture
title: A2A AgentSkill is advertisement metadata, not executor routing
type: concept
---

# A2A AgentSkill is advertisement metadata, not executor routing

In the a2a-sdk, an `AgentSkill` (id · name · description · tags · examples) is **advertisement-only metadata**. It goes into the Agent Card (served at `/.well-known/agent-card.json`) so clients can *discover* capabilities. It does NOT connect to the AgentExecutor — the SDK does not route a skill id to a handler.

Dispatch is entirely up to the executor's single `execute(context, event_queue)`, which inspects the incoming **message** (typically its text) and decides what to do. Card and executor are two separate inputs to the A2A app (`create_agent_card_routes(card)` vs `create_jsonrpc_routes(DefaultRequestHandler(agent_executor=…))`); passing the card to DefaultRequestHandler is reference metadata, not a routing table.

**Consequence / gotcha:** the card is a *promise*, the executor is the *implementation*, and nothing keeps them in sync. They can drift — an agent can advertise a skill it never handles (seen in the test-agent repo: `search-memory` and `get-note` are advertised but have no executor branch). Keeping the card honest is developer discipline.

Contrast: **MCP tools** DO map tool-name → handler function directly; that name→function surface in the test-agent lives on the bridge/MCP side, not on the A2A skill card. Mental model: Agent Card = menu, Executor = kitchen. Related: [[test-agent two A2A agents share a skeleton but diverge in domain engines]].

## Related

- [[test-agent two A2A agents share a skeleton but diverge in domain engines]]

%% ai-graph-start %%

**Related notes:**
- [[ADK to_a2a auto-card is generic; pass agent_card= to keep a rich AgentCard]]
- [[a2a-sdk serves the agent card at agent-card.json (new) or agent.json (old)]]
- [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]]
- [[A2A messagesend returns either a Message or a Task envelope]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]

**Relations:**
- A2A AgentSkill — *is* — advertisement metadata
- A2A AgentSkill — *is not* — executor routing
- A2A AgentSkill — *is defined in* — a2a-sdk
- A2A AgentSkill — *goes into* — Agent Card
- Agent Card — *is served at* — `/.well-known/agent-card.json`
- clients — *discover* — capabilities
- clients — *use* — Agent Card
- SDK — *does not route* — skill id
- skill id — *to* — handler
- Dispatch — *is handled by* — execute(context, event_queue)
- execute(context, event_queue) — *inspects* — message
- A2A app — *uses* — create_agent_card_routes
- A2A app — *uses* — create_jsonrpc_routes
- create_jsonrpc_routes — *uses* — DefaultRequestHandler
- DefaultRequestHandler — *uses* — AgentExecutor
- Agent Card — *is* — reference metadata
- Agent Card — *is not* — routing table
- Agent Card — *is a* — promise
- AgentExecutor — *is the* — implementation
- Agent Card — *can drift from* — AgentExecutor
- test-agent repo — *advertises* — search-memory
- test-agent repo — *advertises* — get-note
- search-memory — *has no* — executor branch
- get-note — *has no* — executor branch
- MCP tools — *map* — tool-name
- tool-name — *to* — handler function
- name→function surface — *lives on* — bridge/MCP side
- name→function surface — *is not on* — Agent Card
- Agent Card — *is analogous to* — menu
- AgentExecutor — *is analogous to* — kitchen
- test-agent two A2A agents share a skeleton but diverge in domain engines — *is related to* — A2A AgentSkill

%% ai-graph-end %%