---
ai_hash: 10f136b958c02c1f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: A1 build 2026-09-08, google-adk 2.8.0
status: seedling
tags:
- google-adk
- gotcha
- pydantic
- agents
title: ADK agent name must be a valid Python identifier
type: lesson
---

# ADK agent name must be a valid Python identifier

Two validation gotchas hit when building custom ADK agents (google-adk 2.8.0), both surfacing as pydantic ValidationErrors:

1. **Agent `name` must be a valid Python identifier.** `BaseAgent(name="knowledge-gathering")` raises *"Node name 'knowledge-gathering' must be a valid Python identifier"* — hyphens are illegal. Use underscores (`knowledge_gathering`). The **A2A card name is separate**: you can still advertise a hyphenated name by passing `agent_card=` (a prebuilt AgentCard) to `to_a2a()`/serve, so the internal agent name and the public skill-card name need not match.

2. **`Event(actions=...)` must be an EventActions, never None.** Building `Event(author=..., content=..., actions=None)` fails validation. When a step emits no state change, OMIT the `actions` kwarg (let it default) rather than passing None; set it to `EventActions(state_delta={...})` only when you actually have a delta.

Context: building a deterministic text-routing root as a custom `BaseAgent` that delegates to sub-agents via `async for ev in self.sub.run_async(ctx): yield ev`. Related: [[Persist ADK session state from a custom agent via Event state_delta]], [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]], [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]].

## Related

- [[Persist ADK session state from a custom agent via Event state_delta]]
- [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]]

%% ai-graph-start %%

**Related notes:**
- [[ADK to_a2a auto-card is generic; pass agent_card= to keep a rich AgentCard]]
- [[ADK sample canonical layout root_agent in agent.py, sub_agents subpackages, workflow agents]]
- [[ADK canonical orchestration SequentialAgent, LlmAgent+AgentTool, or callbacks — not custom BaseAgent]]
- [[ADK LlmAgent with output_schema cannot use tools or transfer to other agents]]
- [[adk webrun discovers agents by importing package.agent.root_agent via AgentLoader]]

%% ai-graph-end %%