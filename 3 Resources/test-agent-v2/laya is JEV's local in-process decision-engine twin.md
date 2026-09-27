---
ai_hash: d6ad4c84dbedd489
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22; github.com/NandhaKishorM/laya; pypi laya 0.3.5
status: seedling
tags:
- laya
- jev
- decision-engine
- test-agent
- system-1
title: laya is JEV's local in-process decision-engine twin
type: term
---

# laya is JEV's local in-process decision-engine twin

laya (PyPI `laya`, Apache-2.0, by convaiinnovations) is an open-source, non-autoregressive **System-1 decision engine** — the local twin of TypeSafe **JEV**. Same three typed primitives: `choice` (classify), `score` (ordinal), `noul` (calibrated P(true)). It is an **in-process torch/transformers library, NOT a server** (`pip install laya`; real deps `torch>=2.0`, `transformers>=4.48`, `huggingface_hub`, `numpy`; Python >=3.10; downloads HF checkpoints at runtime).

Because it drags torch + multi-GB checkpoints, in test-agent-v2 it is wrapped in a one-endpoint FastAPI **sidecar** (`laya_service/`) so the weight lives in ONE container; the `LayaProvider` DecisionProvider (`TPD_DECISION_BACKEND=laya`, `LAYA_URL`) is an HTTP client to it. Result shape (README, UNVERIFIED against a pinned release): `result["answers"][q]["choice"|"score"|"noul"]` + `["confidence"]`/`["probabilities"]`. GOTCHA: the repo README cites `torch 2.14+` / `transformers 5.x` (nonexistent) — the PyPI `requires_dist` (torch>=2.0, transformers>=4.48) is authoritative. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]

%% ai-graph-start %%

**Related notes:**
- [[Low-disk CPU box drop laya, use JEV cloud API for decisions]]
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[Best fully-offline CPU config for test-agent-v2 (qwen2.53b + Turbo)]]
- [[System One Models (Jev) fast type-safe calibrated decision models, not chat LLMs]]
- [[Pluggable LLM via the litellm ModelProvider backend]]

%% ai-graph-end %%