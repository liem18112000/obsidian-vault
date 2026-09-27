---
ai_hash: df5aa589c530480a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: session 2026-08-03
status: seedling
tags:
- bitnami
- dockerhub
- kafka
- imagepull
- gotcha
- manifest-unknown
title: Bitnami 2025 catalog reorg removed pinned bitnami/* version tags
type: lesson
---

# Bitnami 2025 catalog reorg removed pinned bitnami/* version tags

In 2025 Bitnami reorganized their Docker Hub catalog ("Bitnami Secure Images"), and **pinned version tags on `bitnami/*` Docker Hub repos were removed** — e.g. `docker pull bitnami/kafka:3.7` now fails with `manifest unknown` even though it worked before, and `bitnami/kafka:latest` / `bitnami/kafka:3.9` are also gone from the free repo.

Distinguish this from a rate-limit: `manifest unknown` = the tag does not exist (auth/`docker login` won't help); a rate-limit instead shows a slow/stalled *layer download* after the manifest resolves.

Fixes for a pinned old Bitnami version:
- Use the frozen legacy namespace: **`bitnamilegacy/kafka:3.7`** (same image + same `KAFKA_CFG_*` config → drop-in, but deprecated/unmaintained).
- Or switch to the official image: **`apache/kafka:3.7.0`** (maintained, KRaft-capable, but different env-var conventions than Bitnami — needs config rewrite).

Check existence cheaply with `docker manifest inspect <img>` (a HEAD-style check, no layer download).

## Related
- [[kind image pulls stall on Docker Hub rate limits; pre-pull and kind load]]
- [[Customer360 Kubernetes deployment (local kind + GreenNode VKS)]]

%% ai-graph-start %%

**Related notes:**
- [[kind image pulls stall on Docker Hub rate limits; pre-pull and kind load]]
- [[MinIO Docker images live on quay.io not Docker Hub]]
- [[Pin deploys by @sha256 of the TAGGED manifest, not the newest push (buildx multi-arch creates untagged sibling manifests)]]

%% ai-graph-end %%