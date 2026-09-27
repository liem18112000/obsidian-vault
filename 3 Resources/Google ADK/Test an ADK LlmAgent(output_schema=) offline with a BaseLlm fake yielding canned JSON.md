---
ai_hash: 06287fa0ac7e8daf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: test-agent-v2 D15/D16 LlmAgent work, session 2026-09-08
status: seedling
tags:
- google-adk
- llmagent
- output-schema
- testing
- offline
- pydantic
title: Test an ADK LlmAgent(output_schema=) offline with a BaseLlm fake yielding canned
  JSON
type: howto
---

# Test an ADK LlmAgent(output_schema=) offline with a BaseLlm fake yielding canned JSON

ADK has no built-in fake/echo LLM for offline tests. To test an `LlmAgent(output_schema=SomeBaseModel)` without a network call, subclass `google.adk.models.base_llm.BaseLlm` and yield ONE canned response:

```python
from google.adk.models.base_llm import BaseLlm
from google.adk.models.llm_response import LlmResponse
from google.genai import types

class FakeStructuredModel(BaseLlm):
    model: str = "fake"; canned: str = "{}"; calls: list = Field(default_factory=list)
    async def generate_content_async(self, llm_request, stream=False):
        self.calls.append(llm_request)
        yield LlmResponse(content=types.Content(role="model", parts=[types.Part(text=self.canned)]))
```

Build the agent with `build_agent(model=FakeStructuredModel(canned=json))` — give the production factory a `model=` injection seam. Key facts:
- ADK realizes `output_schema` for non-Gemini models by **instructing JSON + validating the reply** — so a VALID canned JSON string exercises the real save path (ADK writes the validated dict to `session.state[output_key]`), and a JUNK string raises `pydantic.ValidationError` inside `run_async`, which your agent should catch to hit its degrade/fallback path. You test both branches by choosing `canned`.
- Recording every request in `.calls` lets a test assert **call-count** invariants — e.g. "default path makes exactly 1 LLM call" (flag/budget gates) vs zero when a feature flag is off.
- Do NOT rely on the real `agent_model()` in tests — it returns a network-backed model; and if the offline env blanks the model creds, `agent_model()` may return None (→ your fallback) or a model that would hit the network. Inject the fake explicitly.
- To drive one agent: a throwaway `Runner(app_name, agent, InMemorySessionService)`; seed inputs via `create_session(..., state={key: value})` and read `session.state[output_key]` after. Do not parse event text.

Used across KGA/TPD LlmAgent conversions (test-agent-v2). See [[ADK to_a2a auto-card is generic; pass agent_card= to keep a rich AgentCard]].

## Related

- [[ADK to_a2a builds A2A routes on ASGI lifespan startup, not at construction]]

%% ai-graph-start %%

**Related notes:**
- [[ADK LlmAgent with output_schema cannot use tools or transfer to other agents]]
- [[Hermetic E2E test of an LLM agent mock only the SDK boundary, inject the prompt snapshot]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[Concurrent in-process ADK Runners return simultaneously-empty output]]
- [[Pluggable LLM via the litellm ModelProvider backend]]

%% ai-graph-end %%