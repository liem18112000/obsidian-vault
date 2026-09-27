---
title: "ADK to_a2a auto-card is generic; pass agent_card= to keep a rich AgentCard"
created: 2026-09-08
type: lesson
status: seedling
source: "test-agent-v2 gateway cutover, session 2026-09-08"
tags: [google-adk, a2a, agent-card, decision]
---

# ADK to_a2a auto-card is generic; pass agent_card= to keep a rich AgentCard

ADK's `to_a2a(agent, runner=…)` auto-generates an A2A `AgentCard` from the agent, and it is **generic**: `name` = the agent's Python name (e.g. `knowledge_gathering`, underscores), `version` = `0.0.1`, `description` = `"An ADK Agent"`, and skills are auto-derived with odd ids (`gather_gather`, `<name>-sub-agents`).

**To preserve a hand-authored card**, pass `agent_card=` — `to_a2a(root, runner=runner, agent_card=my_card)`. This is the framework's sanctioned override seam, so keeping a curated `AgentCard` (real name, description, skill ids) is *reuse of an extension point*, not reinventing the wheel. Passing `agent_card=None` keeps the auto card.

Applies when the A2A card is a real surface (e.g. an MCP gateway that fetches `/.well-known/agent-card.json` to show agent names/skills). See [[ADK to_a2a builds A2A routes on ASGI lifespan startup, not at construction]].

## Related

- [[ADK to_a2a builds A2A routes on ASGI lifespan startup]]
- [[not at construction]]
