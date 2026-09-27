---
ai_hash: 6a7f1c975f98ec96
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities:
- RAGAS
- OpenAI
- LLM
- Faithfulness
- Answer/Response Relevancy
- Claude-on-Vertex
- ADK
- ModelProvider
- OPENAI_API_KEY
- LangchainLLMWrapper
- embeddings equivalent
- test-agent-v2
- test_evaluation
- ragas_judge.judge
- build_ragas_llm()
- I8
- EvalMetric
- LlmAgent
- embeddings
- judge
- configured provider
- test harness
- deterministic scorer
- ragas.evaluate
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

- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric]]
- [[not LlmAgent]]

%% ai-graph-start %%

**Related notes:**
- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]
- [[LLM-as-a-judge biases position, verbosity, self-enhancement]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[Pluggable LLM via the litellm ModelProvider backend]]
- [[A custom CI secret name must be passed to an SDK explicitly — SDKs only auto-read their fixed env var]]

**Relations:**
- RAGAS — *defaults to* — OpenAI
- RAGAS — *defaults to* — embeddings
- RAGAS — *evaluates* — LLM-judged metrics
- LLM-judged metrics — *include* — Faithfulness
- LLM-judged metrics — *include* — Answer/Response Relevancy
- Claude-on-Vertex — *is a type of* — model
- ADK — *provides* — ModelProvider
- ModelProvider — *can source* — Claude-on-Vertex
- RAGAS — *requires* — OPENAI_API_KEY
- LangchainLLMWrapper — *wraps* — LLM
- embeddings equivalent — *wraps* — embeddings
- RAGAS — *has function* — evaluate
- evaluate — *accepts argument* — llm
- evaluate — *accepts argument* — embeddings
- test-agent-v2 — *contains* — test_evaluation
- test_evaluation — *uses* — ragas_judge.judge
- ragas_judge.judge — *accepts argument* — llm
- ragas_judge.judge — *accepts argument* — embeddings
- build_ragas_llm() — *is a* — provider-sourced factory
- build_ragas_llm() — *is related to* — I8
- ADK — *is reused via* — custom EvalMetric
- LlmAgent — *is a negative case for* — deterministic scorer
- judge — *is built from* — configured provider
- embeddings — *is built from* — configured provider
- test harness — *injected* — llm
- test harness — *injected* — embeddings
- RAGAS — *uses* — judge
- RAGAS — *uses* — embeddings
- ragas.evaluate — *is a function of* — RAGAS
- judge — *is* — provider-sourced

%% ai-graph-end %%