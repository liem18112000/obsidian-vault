---
ai_hash: 58e20d49ba6b4e4d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: session 2026-09-08
status: seedling
tags:
- google-adk
- llmagent
- output-schema
- pydantic
- gotcha
title: ADK LlmAgent with output_schema cannot use tools or transfer to other agents
type: lesson
---

# ADK LlmAgent with output_schema cannot use tools or transfer to other agents

In Google ADK, an `LlmAgent` (a.k.a. `Agent`) that sets `output_schema` (a pydantic `BaseModel` for structured/JSON replies) becomes a **structured-reply leaf**: it **cannot** also declare `tools=` and **cannot** transfer control to other agents (no auto-flow / sub-agent transfer). Setting `output_schema` is mutually exclusive with tool use and delegation.

Consequences when designing:
- Fine for **pure enumerator/extractor** agents (one prompt → validated JSON, no tools needed).
- If an agent must BOTH call tools AND return a typed object, you must split it — e.g. a tool-using agent whose result is post-validated, or a separate structured-output agent downstream.
- For non-Gemini models (e.g. Claude via LiteLlm), ADK enforces `output_schema` by **instructing JSON + validating the reply** (prompt-enforced), not Gemini controlled-generation — so keep "return ONLY JSON" wording and enough `max_tokens` for the payload.

Calling such a leaf from a custom `BaseAgent`: seed `ctx.session.state` with the inputs, set an `output_key`, run `async for _ in agent.run_async(ctx): pass`, then read the validated result from `ctx.session.state[output_key]` (re-wrap with the pydantic model) — do not parse the event text.

Related: [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]].

## Related

- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]

%% ai-graph-start %%

**Related notes:**
- [[Test an ADK LlmAgent(output_schema=) offline with a BaseLlm fake yielding canned JSON]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[ADK canonical orchestration SequentialAgent, LlmAgent+AgentTool, or callbacks — not custom BaseAgent]]
- [[Concurrent in-process ADK Runners return simultaneously-empty output]]
- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]

%% ai-graph-end %%