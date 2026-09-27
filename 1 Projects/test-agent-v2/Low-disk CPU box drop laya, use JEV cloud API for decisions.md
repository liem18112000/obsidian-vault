---
title: "Low-disk CPU box: drop laya, use JEV cloud API for decisions"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22"
tags: [test-agent, jev, laya, decision-provider, low-disk, cpu]
---

# Low-disk CPU box: drop laya, use JEV cloud API for decisions

DECISION (2026-09-22): on a disk-constrained CPU-only workstation, the laya local decision sidecar was DROPPED because its torch dependency cannot fit (CPU wheel blocked by proxy hash mismatch; PyPI torch pulls ~5-6GB CUDA wheels that filled the disk to 0 and crashed the Rancher/WSL Docker backend — see [[Ollamas 3.5GB CUDA image can corrupt a low-disk RancherWSL Docker store]] and [[PyTorch CPU index gives pip hash mismatch behind a proxy — use PyPI torch]]). Replacement: the **JEV cloud API** via the existing `jev` DecisionProvider (`TPD_DECISION_BACKEND=jev` + `TYPESAFE_API_KEY`), which is a REMOTE call → zero local compute/disk — ideal for a tiny box. Removed the `laya` service + its `depends_on` from compose. If `TYPESAFE_API_KEY` is blank, JevProvider.is_configured()=false → decisions gracefully fall back to the local LLM. This is the pattern: heavy local ML deps (torch) are the enemy on low-disk/CPU; prefer a remote API for the model AND the decision engine. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
