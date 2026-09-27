---
ai_hash: cb001c71a48a450f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities: []
source: session 2026-09-21 (agent live-eval, gpt-5.6-luna)
status: seedling
tags:
- llm
- litellm
- openai
- reasoning-models
- temperature
- gotcha
- customer360-agent
title: Reasoning models reject temperature != 1; use litellm.drop_params for multi-model
  agents
type: gotcha
---

# Reasoning models reject temperature != 1; use litellm.drop_params for multi-model agents

OpenAI **reasoning** models (gpt-5.x family, e.g. `gpt-5.6-luna`) reject `temperature != 1` while reasoning is active — LiteLLM surfaces this as `litellm.UnsupportedParamsError: ... doesn't support temperature=0.4 while reasoning is active`. Any code that hard-codes a sampling `temperature` (a very common default like 0.4/0.7) will 502 on EVERY call once the configured model is switched to a reasoning model.

This bit customer360-agent: `ai_providers/base.py` set `params = {"temperature": 0.4}` unconditionally. It worked with `gpt-4o-mini` but broke completely when `LEO_OPENAI_MODEL_NAME` was `gpt-5.6-luna`.

**Fix for a provider/model-agnostic agent:** set `litellm.drop_params = True` (global, or `drop_params=True` per call). LiteLLM then silently drops any parameter the target model doesn't support instead of raising — temperature stays for models that allow it, is dropped for reasoning models (which then use their forced default of 1). One line, keeps the agent working across the whole model matrix.

Meta-lesson: a hermetic mock test cannot catch this — the mock never validates params against a real model. A **live** eval against the actually-configured model is what surfaced it. Run one before trusting a model swap.

Also observed (same run): the reasoning model cost ~4x and ran ~2x slower than gpt-4o-mini, but produced correct future-dated campaigns and full locale-appropriate (vi-VN) output, where gpt-4o-mini invented 2023 dates.

## Related

- [[Hermetic E2E test of an LLM agent: mock only the SDK boundary]]
- [[inject the prompt snapshot]]

%% ai-graph-start %%

**Related notes:**
- [[Hermetic E2E test of an LLM agent mock only the SDK boundary, inject the prompt snapshot]]
- [[Pluggable LLM via the litellm ModelProvider backend]]
- [[LLM-as-reranker JSON truncation budget max_tokens for pretty-printed output, not just element count]]
- [[Customer 360 uses two model layers LLM for Generate, System One for StructureDecide]]
- [[Best fully-offline CPU config for test-agent-v2 (qwen2.53b + Turbo)]]

%% ai-graph-end %%