---
title: "Pluggable LLM via the litellm ModelProvider backend"
created: 2026-09-22
type: concept
status: seedling
source: "session 2026-09-22"
tags: [test-agent, litellm, llm, adk, model-provider]
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
