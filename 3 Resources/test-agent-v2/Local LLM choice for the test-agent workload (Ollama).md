---
title: "Local LLM choice for the test-agent workload (Ollama)"
created: 2026-09-22
type: reference
status: seedling
source: "session 2026-09-22"
tags: [ollama, local-llm, qwen, nemotron, llm, test-agent]
---

# Local LLM choice for the test-agent workload (Ollama)

For running the test-agent-v2 pipeline on a LOCAL model (via Ollama + the litellm provider, `LITELLM_MODEL=ollama/<m>`), the workload needs: strong instruction-following, reliable **JSON/schema** output (the scenario generator), **long context** (the knowledge pack), and some reasoning (interrogation/judging) — capability profile matters more than raw size.

Picks:
- **Qwen2.5/Qwen3 (14B–32B)** = best open family here (top JSON-schema adherence + 128k context). `qwen2.5:14b` is the realistic default (12–16GB VRAM); `qwen3:32b` for 24GB.
- **NVIDIA Nemotron**: `nemotron` = Llama-3.1-Nemotron-70B-Instruct (RLHF, reasoning/agentic, ~24GB q4); `nemotron-mini` (4B) too weak; Nano-8B good on one GPU.
- Alternatives: Llama-3.3-70B (24GB+), Mistral-Small-24B / Phi-4-14B (mid), Llama-3.1-8B / Qwen2.5-7B (8GB).

GOTCHA: the compose default `llama3.2` (3B) is a WIRING placeholder — far too weak for real test-plan quality; bump it. Switch = set `LITELLM_MODEL=ollama/<m>` + `OLLAMA_CHAT_MODEL=<m>` (ollama-init pulls it). Embeddings: nomic-embed-text ok; bge-m3 / mxbai-embed-large stronger. Tag names/sizes on ollama.com/library move fast — verify before pulling (assistant cutoff Jan 2026). See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]] and [[Pluggable LLM via the litellm ModelProvider backend]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[Pluggable LLM via the litellm ModelProvider backend]]
