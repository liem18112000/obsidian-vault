---
ai_hash: 74feefdeab762d9b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: session 2026-09-08
status: seedling
tags:
- ragas
- evaluation
- openai
- llm-judge
- gotcha
- google-adk
title: RAGAS silently defaults to OpenAI when no llm is injected — pass a provider-sourced
  judge
type: gotcha
---

# RAGAS silently defaults to OpenAI when no llm is injected — pass a provider-sourced judge

RAGAS (`ragas.evaluate`) **silently defaults to OpenAI** (and OpenAI embeddings) for its LLM-judged metrics — Faithfulness, Answer/Response Relevancy, etc. — if you do not pass an explicit `llm=`/`embeddings=`. So a project standardized on a different model (e.g. Claude-on-Vertex via an ADK `ModelProvider`) will, unless it injects its own judge, silently route judged-eval scoring to OpenAI, needing an `OPENAI_API_KEY` and using the wrong model.

Fix: always build the judge + embeddings from your configured provider and pass them in — wrap the model in RAGAS's `LangchainLLMWrapper` (and the embeddings equivalent) and hand them to `evaluate(..., llm=…, embeddings=…)`. In test-agent-v2 `test_evaluation`, `ragas_judge.judge` already accepts `llm=`/`embeddings=`; the gap was that only the test harness injected them, so the intended fix is a provider-sourced `build_ragas_llm()` factory (I8).

General rule: any third-party eval/LLM library that "just works" without a model argument has a hidden default provider — check and pin it, or you get silent cross-provider drift.

Related: [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]].

## Related

- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]

%% ai-graph-start %%

**Related notes:**
- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]
- [[LLM-as-a-judge biases position, verbosity, self-enhancement]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[Pluggable LLM via the litellm ModelProvider backend]]
- [[Judge Calibration and Canary Seeds]]

%% ai-graph-end %%