---
ai_hash: b9db057667f735d9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- local-llm
- quantization
- sonnet
- litellm
- expectation-setting
title: A 2-4GB local model cannot match Sonnet 5 — plug the real API instead
type: lesson
---

# A 2-4GB local model cannot match Sonnet 5 — plug the real API instead

REALITY CHECK / expectation-setting: you cannot get Claude-Sonnet-5-level quality from a model that fits 2-4GB RAM. 2-4GB (Q4) caps you at ~a 3B-param model (3B~=2GB, 7B~=4.4GB which wont fit with KV cache + overhead); Sonnet 5 is a frontier model orders of magnitude larger. The gap is fundamental — quantization/prompting does not close it.

The RIGHT move when someone wants "Sonnet quality on a tiny box": the test-agent model is PLUGGABLE, so point litellm at the **real Claude API** (`LITELLM_MODEL=anthropic/claude-sonnet-5` + `LITELLM_API_KEY`). The weights arent local -> ~0 RAM for the model; "local run" just means the stack runs on your machine while the model call is remote. Or a HYBRID via the providers fast tier: `LITELLM_MODEL=anthropic/claude-sonnet-5` for heavy generation + `LITELLM_MODEL_FAST=ollama/qwen2.5:3b` for cheap classify/judge. Best fully-offline 2-4GB pick = qwen2.5:3b (~2GB) or phi4-mini (3.8B) — OK for wiring/smoke, weak for real test plans. See [[Local LLM choice for the test-agent workload (Ollama)]] and [[Pluggable LLM via the litellm ModelProvider backend]].

## Related

- [[Local LLM choice for the test-agent workload (Ollama)]]
- [[Pluggable LLM via the litellm ModelProvider backend]]

%% ai-graph-start %%

**Related notes:**
- [[Best fully-offline CPU config for test-agent-v2 (qwen2.53b + Turbo)]]
- [[Local LLM choice for the test-agent workload (Ollama)]]
- [[Pluggable LLM via the litellm ModelProvider backend]]
- [[Best Ollama models for CPU-only coding and research on a thin laptop]]
- [[Local RAG stack for a 1 vCPU 2 GB box e5 + bge-reranker + Qwen 0.5B]]

%% ai-graph-end %%