---
ai_hash: 0deaf7abc3395c76
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- test-agent
- litellm
- llm
- adk
- model-provider
title: Pluggable LLM via the litellm ModelProvider backend
type: concept
---

# Pluggable LLM via the litellm ModelProvider backend

test-agent-v2 reaches its model through a `ModelProvider` port (`common/adk/providers/`). The generic **`litellm`** backend makes any LLM pluggable without GCP: select it with `TESTAGENT_MODEL_BACKEND=litellm`, then set `LITELLM_MODEL` (+ optional `LITELLM_API_BASE`, `LITELLM_API_KEY`, `LITELLM_MODEL_FAST`).

It wraps ADK `LiteLlm` (for `LlmAgent`) and `litellm.completion` (for the sync engine text path), so it speaks whatever LiteLLM routes:
- `anthropic/claude-sonnet-4-5` — real Claude, API key, no GCP
- `ollama/llama3.1` + `LITELLM_API_BASE=http://host.docker.internal:11434` — fully local
- `openai/<served-name>` + a local `LITELLM_API_BASE` — LM Studio / vLLM / llama.cpp / LocalAI

Key detail: it passes `drop_params=True`, so params a local OpenAI-compatible server does not understand (thinking, cache_control, etc.) are silently dropped instead of erroring — that is what lets arbitrary local models plug in without per-server special-casing. Registered in `providers/__init__._REGISTRY` alongside the default `claude` (VertexClaude). `is_configured()` is true iff `LITELLM_MODEL` is set, which drives the LLM-vs-heuristic gate. Part of [[Run test-agent-v2 locally with docker-compose (no GCP)]].

## Related

- [[Run test-agent-v2 locally with docker-compose (no GCP)]]

%% ai-graph-start %%

**Related notes:**
- [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]]
- [[A 2-4GB local model cannot match Sonnet 5 — plug the real API instead]]
- [[Run test-agent-v2 locally with docker-compose (no GCP)]]
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[Best fully-offline CPU config for test-agent-v2 (qwen2.53b + Turbo)]]

%% ai-graph-end %%