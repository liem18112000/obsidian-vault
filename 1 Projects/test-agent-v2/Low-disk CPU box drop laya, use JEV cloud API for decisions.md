---
ai_hash: 771c7f66e4e913e0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities:
- Low-disk CPU box
- laya
- JEV cloud API
- torch
- PyPI
- CUDA wheels
- Rancher/WSL Docker backend
- Ollama
- '`jev` DecisionProvider'
- '`TPD_DECISION_BACKEND`'
- '`TYPESAFE_API_KEY`'
- local LLM
- compose
- MinIO
- PG
- Redis
- PubSub
- test-agent-v2
- CPU wheel
- proxy hash mismatch
- Rancher/WSL Docker store
- '`laya` service'
- remote API
- model
- decision engine
- workstation
- local decision sidecar
- remote call
- heavy local ML deps
- low-disk/CPU environment
- Ollama's 3.5GB CUDA image
- PyPI torch
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

DECISION (2026-09-22): on a disk-constrained CPU-only workstation, the laya local decision sidecar was DROPPED because its torch dependency cannot fit (CPU wheel blocked by proxy hash mismatch; PyPI torch pulls ~5-6GB CUDA wheels that filled the disk to 0 and crashed the Rancher/WSL Docker backend — see [[Ollama's 3.5GB CUDA image can corrupt a low-disk RancherWSL Docker store|Ollamas 3.5GB CUDA image can corrupt a low-disk RancherWSL Docker store]] and [[PyTorch CPU index gives pip hash mismatch behind a proxy — use PyPI torch]]). Replacement: the **JEV cloud API** via the existing `jev` DecisionProvider (`TPD_DECISION_BACKEND=jev` + `TYPESAFE_API_KEY`), which is a REMOTE call → zero local compute/disk — ideal for a tiny box. Removed the `laya` service + its `depends_on` from compose. If `TYPESAFE_API_KEY` is blank, JevProvider.is_configured()=false → decisions gracefully fall back to the local LLM. This is the pattern: heavy local ML deps (torch) are the enemy on low-disk/CPU; prefer a remote API for the model AND the decision engine. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

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
- Low-disk CPU box — *is a type of* — workstation
- laya — *is a* — local decision sidecar
- laya — *depends on* — torch
- torch — *incompatible with* — Low-disk CPU box
- torch — *downloads* — CUDA wheels
- CUDA wheels — *consumed disk space on* — Rancher/WSL Docker backend
- Ollama's 3.5GB CUDA image — *can corrupt* — Rancher/WSL Docker store
- JEV cloud API — *replaces* — laya
- JEV cloud API — *accessed via* — `jev` DecisionProvider
- `jev` DecisionProvider — *configured by* — `TPD_DECISION_BACKEND`
- `jev` DecisionProvider — *requires* — `TYPESAFE_API_KEY`
- JEV cloud API — *is a* — remote call
- JEV cloud API — *requires* — zero local compute/disk
- JEV cloud API — *is ideal for* — Low-disk CPU box
- `laya` service — *removed from* — compose
- `laya` service — *had dependency in* — compose
- `TYPESAFE_API_KEY` — *blank leads to fallback to* — local LLM
- heavy local ML deps — *are problematic on* — Low-disk CPU box
- remote API — *preferred for* — model
- remote API — *preferred for* — decision engine
- torch — *is a type of* — heavy local ML deps
- laya — *is a type of* — heavy local ML deps
- test-agent-v2 — *uses* — MinIO
- test-agent-v2 — *uses* — PG
- test-agent-v2 — *uses* — Redis
- test-agent-v2 — *uses* — PubSub
- test-agent-v2 — *uses* — Ollama
- test-agent-v2 — *uses* — laya
- PyPI — *provides* — torch
- CPU wheel — *blocked by* — proxy hash mismatch
- Rancher/WSL Docker store — *is part of* — Rancher/WSL Docker backend
- JEV cloud API — *is a* — decision engine
- laya — *is a* — decision engine
- local LLM — *is a* — decision engine
- Low-disk CPU box — *is a type of* — low-disk/CPU environment
- PyPI torch — *pulls* — CUDA wheels
- JEV cloud API — *is a type of* — remote API

%% ai-graph-end %%