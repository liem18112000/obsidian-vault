---
ai_hash: 14680805db80c5d7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- pip
- pytorch
- docker
- buildkit
- proxy
- gotcha
- dns
title: PyTorch CPU index gives pip hash mismatch behind a proxy — use PyPI torch
type: lesson
---

# PyTorch CPU index gives pip hash mismatch behind a proxy — use PyPI torch

GOTCHA (constrained/proxied corporate network): `pip install --index-url https://download.pytorch.org/whl/cpu torch` failed in a Docker build with `ERROR: THESE PACKAGES DO NOT MATCH THE HASHES FROM THE REQUIREMENTS FILE ... Expected sha256 <a> ... Got <b>`. Cause: the network path to the custom PyTorch index (download.pytorch.org) is proxy/cache-rewritten, so the wheel bytes differ from the index-declared hash → pip aborts. Regular **PyPI (files.pythonhosted.org)** worked fine through the same network (other images installed). FIX: install torch from default PyPI (let it come as a normal dependency) instead of the pytorch CPU index. TRADE: PyPIs linux torch wheel bundles CUDA libs (bigger image, unused on CPU) — accepted for a path that actually installs. Also: BuildKit build-container DNS can transiently break right after a `wsl --shutdown`/Rancher restart (`[Errno -2] Name or service not known`) while the daemon/runtime DNS still resolves — retry once the engine is stable, or build with `--network=host`. See [[Ollama's 3.5GB CUDA image can corrupt a low-disk RancherWSL Docker store|Ollamas 3.5GB CUDA image can corrupt a low-disk RancherWSL Docker store]].

## Related

- [[Ollama's 3.5GB CUDA image can corrupt a low-disk RancherWSL Docker store]]

%% ai-graph-start %%

**Related notes:**
- [[Ollama's 3.5GB CUDA image can corrupt a low-disk RancherWSL Docker store]]
- [[Low-disk CPU box drop laya, use JEV cloud API for decisions]]

%% ai-graph-end %%