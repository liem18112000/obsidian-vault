---
ai_hash: 8f41ff8fcec53471
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities:
- test-agent-v2
- qwen2.5:3b
- Turbo
- LITELLM_MODEL
- ollama/qwen2.5:3b
- qwen2.5:1.5b
- TESTAGENT_TURBO
- TPD_LLM_DETAIL
- laya
- TPD_DECISION_BACKEND
- CPU
- model budget
- JSON/instructions
- LLM gates
- long generations
- model
- wiring
- weak plans
- Sonnet-5 quality
- Sonnet-5
- whole containerized stack
- Postgres
- MinIO
- Redis
- Ollama
- laya-torch
- agents
- worker
- machine RAM
- A 2-4GB local model cannot match Sonnet 5 — plug the real API instead
- Local LLM choice for the test-agent workload (Ollama)
source: session 2026-09-22
status: seedling
tags:
- test-agent
- ollama
- cpu
- offline
- qwen
- laya
title: Best fully-offline CPU config for test-agent-v2 (qwen2.5:3b + Turbo)
type: howto
---

# Best fully-offline CPU config for test-agent-v2 (qwen2.5:3b + Turbo)

For a fully-offline, CPU-only, ~2-4GB (model budget) run of test-agent-v2, the tuned `.env.compose`: `LITELLM_MODEL=ollama/qwen2.5:3b` (best ~3B for JSON/instructions at ~2GB q4; qwen2.5:1.5b for more speed), `TESTAGENT_TURBO=1` (fewer/faster LLM gates — critical on CPU where long generations are slow), leave `TPD_LLM_DETAIL` unset (1 LLM call). laya (`TPD_DECISION_BACKEND=laya`) is KEPT even on CPU: it is a small (~300-420M) NON-autoregressive model doing one forward pass per decision, so it is CPU-viable (~100s of ms), unlike the autoregressive LLM.

HONEST CEILING: this only makes the *model* fit ~2-4GB and only proves wiring / gives weak plans; it does NOT approach Sonnet-5 quality (see [[A 2-4GB local model cannot match Sonnet 5 — plug the real API instead]]). And the *whole containerized stack* (Postgres+MinIO+Redis+Ollama+laya-torch+5 agents+worker) needs ~8-12GB TOTAL machine RAM regardless of the model — 2-4GB TOTAL will not boot it. See [[Local LLM choice for the test-agent workload (Ollama)]].

## Related

- [[A 2-4GB local model cannot match Sonnet 5 — plug the real API instead]]
- [[Local LLM choice for the test-agent workload (Ollama)]]

%% ai-graph-start %%

**Related notes:**
- [[A 2-4GB local model cannot match Sonnet 5 — plug the real API instead]]
- [[Local LLM choice for the test-agent workload (Ollama)]]
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[Best Ollama models for CPU-only coding and research on a thin laptop]]
- [[Low-disk CPU box drop laya, use JEV cloud API for decisions]]

**Relations:**
- test-agent-v2 — *uses* — qwen2.5:3b
- test-agent-v2 — *uses* — Turbo
- LITELLM_MODEL — *is set to* — ollama/qwen2.5:3b
- ollama/qwen2.5:3b — *is* — qwen2.5:3b
- qwen2.5:3b — *is* — ~3B
- qwen2.5:3b — *is* — ~2GB q4
- qwen2.5:3b — *is best for* — JSON/instructions
- qwen2.5:1.5b — *is for* — more speed
- TESTAGENT_TURBO — *is set to* — 1
- Turbo — *enables* — fewer/faster LLM gates
- Turbo — *is critical on* — CPU
- long generations — *are slow on* — CPU
- TPD_LLM_DETAIL — *is* — unset
- laya — *is* — TPD_DECISION_BACKEND
- laya — *is* — CPU-viable
- laya — *is* — NON-autoregressive model
- laya — *is* — ~300-420M
- laya — *does* — one forward pass per decision
- model — *fits* — ~2-4GB
- model — *proves* — wiring
- model — *gives* — weak plans
- model — *does NOT approach* — Sonnet-5 quality
- Sonnet-5 — *is related to* — A 2-4GB local model cannot match Sonnet 5 — plug the real API instead
- whole containerized stack — *includes* — Postgres
- whole containerized stack — *includes* — MinIO
- whole containerized stack — *includes* — Redis
- whole containerized stack — *includes* — Ollama
- whole containerized stack — *includes* — laya-torch
- whole containerized stack — *includes* — 5 agents
- whole containerized stack — *includes* — worker
- whole containerized stack — *needs* — ~8-12GB TOTAL machine RAM
- 2-4GB TOTAL — *will not boot* — whole containerized stack
- Ollama — *is related to* — Local LLM choice for the test-agent workload (Ollama)
- test-agent-v2 — *is* — fully-offline
- test-agent-v2 — *is* — CPU-only
- test-agent-v2 — *has* — model budget
- model budget — *is* — ~2-4GB

%% ai-graph-end %%