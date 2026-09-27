---
ai_hash: d3c81764bbdb7a22
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities:
- Low-disk CPU box
- laya
- JEV cloud API
- torch
- local decision sidecar
- CPU wheel
- proxy hash mismatch
- PyPI
- CUDA wheels
- Rancher/WSL Docker
- disk
- jev DecisionProvider
- TPD_DECISION_BACKEND
- TYPESAFE_API_KEY
- compose
- JevProvider.is_configured()
- local LLM
- remote API
- model
- decision engine
- heavy local ML deps
- Ollamas 3.5GB CUDA image
- Rancher/WSL Docker store
- PyTorch CPU index
- pip hash mismatch
- Full local-parity stack
- test-agent-v2
- MinIO
- PG
- Redis
- PubSub
- Ollama
- decisions
source: session 2026-09-22
status: seedling
tags:
- test-agent
- jev
- laya
- decision-provider
- low-disk
- cpu
title: 'Low-disk CPU box: drop laya, use JEV cloud API for decisions'
type: lesson
---

# Low-disk CPU box: drop laya, use JEV cloud API for decisions

DECISION (2026-09-22): on a disk-constrained CPU-only workstation, the laya local decision sidecar was DROPPED because its torch dependency cannot fit (CPU wheel blocked by proxy hash mismatch; PyPI torch pulls ~5-6GB CUDA wheels that filled the disk to 0 and crashed the Rancher/WSL Docker backend — see [[Ollamas 3.5GB CUDA image can corrupt a low-disk RancherWSL Docker store]] and [[PyTorch CPU index gives pip hash mismatch behind a proxy — use PyPI torch]]). Replacement: the **JEV cloud API** via the existing `jev` DecisionProvider (`TPD_DECISION_BACKEND=jev` + `TYPESAFE_API_KEY`), which is a REMOTE call → zero local compute/disk — ideal for a tiny box. Removed the `laya` service + its `depends_on` from compose. If `TYPESAFE_API_KEY` is blank, JevProvider.is_configured()=false → decisions gracefully fall back to the local LLM. This is the pattern: heavy local ML deps (torch) are the enemy on low-disk/CPU; prefer a remote API for the model AND the decision engine. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]

%% ai-graph-start %%

**Related notes:**
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[laya is JEV's local in-process decision-engine twin]]
- [[Best fully-offline CPU config for test-agent-v2 (qwen2.53b + Turbo)]]
- [[Ollama's 3.5GB CUDA image can corrupt a low-disk RancherWSL Docker store]]
- [[Local LLM choice for the test-agent workload (Ollama)]]

**Relations:**
- Low-disk CPU box — *drops* — laya
- Low-disk CPU box — *uses* — JEV cloud API
- laya — *is a* — local decision sidecar
- laya — *has dependency* — torch
- torch — *cannot fit on* — Low-disk CPU box
- torch — *CPU wheel blocked by* — proxy hash mismatch
- PyPI — *distributes* — torch
- PyPI torch — *downloads* — CUDA wheels
- CUDA wheels — *filled* — disk
- disk — *is part of* — Rancher/WSL Docker
- JEV cloud API — *replaces* — laya
- JEV cloud API — *accessed via* — jev DecisionProvider
- jev DecisionProvider — *uses config* — TPD_DECISION_BACKEND
- jev DecisionProvider — *uses config* — TYPESAFE_API_KEY
- JEV cloud API — *is ideal for* — Low-disk CPU box
- laya service — *removed from* — compose
- laya service — *had dependency in* — compose
- JevProvider.is_configured() — *depends on* — TYPESAFE_API_KEY
- decisions — *fall back to* — local LLM
- heavy local ML deps — *are problematic on* — Low-disk CPU box
- remote API — *preferred for* — model
- remote API — *preferred for* — decision engine
- Ollamas 3.5GB CUDA image — *can corrupt* — Rancher/WSL Docker store
- PyTorch CPU index — *causes* — pip hash mismatch
- Full local-parity stack — *for* — test-agent-v2
- Full local-parity stack — *includes* — MinIO
- Full local-parity stack — *includes* — PG
- Full local-parity stack — *includes* — Redis
- Full local-parity stack — *includes* — PubSub
- Full local-parity stack — *includes* — Ollama
- Full local-parity stack — *includes* — laya

%% ai-graph-end %%