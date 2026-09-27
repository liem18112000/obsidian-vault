---
title: "Ollama's 3.5GB CUDA image can corrupt a low-disk Rancher/WSL Docker store"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22"
tags: [docker, ollama, wsl, rancher-desktop, disk, gotcha, cpu]
---

# Ollama's 3.5GB CUDA image can corrupt a low-disk Rancher/WSL Docker store

GOTCHA: `ollama/ollama:latest` is ~3.5GB, almost entirely bundled **CUDA/ROCm GPU libraries** (`mlx_cuda_v13/libcusparse.so...`) that are USELESS on a CPU-only host. Pulling+extracting it needs ~7GB transient. On a low-free-disk Windows host (seen: C: 12GB free, Rancher Desktop WSL data vhdx already 61GB), the extraction filled the WSL ext4 volume and **corrupted the Docker store**: the engine then throws `input/output error` on containerd `meta.db`, buildkit `metadata_v2.db`, and even `overlay2/.../lower` READS (read failures = fs corruption, not just ENOSPC). `docker ps` may still work (cached) while every write/pull fails.

RECOVERY (host-level; CLI cant repair it): quit Rancher Desktop -> `wsl --shutdown` (unmount so ext4 journal-recovers) -> free host disk (20-30GB) -> reopen Rancher, wait for green -> `docker system prune -a --volumes` to clear the corrupted/bloated store. PREVENTION on CPU-only/low-disk: run **Ollama host-native** (`ollama serve` on the host; containers reach `host.docker.internal:11434`) instead of the container — the litellm provider supports it and it avoids the 3.5GB image entirely. See [[Best fully-offline CPU config for test-agent-v2 (qwen2.53b + Turbo)]] and [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Best fully-offline CPU config for test-agent-v2 (qwen2.53b + Turbo)]]
