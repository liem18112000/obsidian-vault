---
ai_hash: 5d2c0e1a16d3ee2a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities: []
source: session 2026-09-05 (AI Chat API vServer deploy)
status: seedling
tags:
- rag
- docker-compose
- deployment
- vserver
- healthcheck
- ops
title: Build a RAG index as an explicit deploy step, run a serve-only container
type: technique
---

# Build a RAG index as an explicit deploy step, run a serve-only container

When containerizing a RAG (or any index-building) service, DONT build the index in the containers startup CMD (`enrich && serve`). A full index build can take minutes (e.g. embedding + per-doc LLM extraction), during which the container is up but not serving — the healthcheck is red, and orchestrators may treat it as failed.

## Pattern: split index-build from serve
- Container `command:` is **serve-only** (`uvicorn ...`), overriding the images combined CMD.
- The deploy script builds/refreshes the index as an explicit one-shot step into a **persisted volume**, then starts the server:
  ```bash
  docker compose -p $P build
  docker compose -p $P run --rm svc python -m enrich   # writes index into the named volume
  docker compose -p $P up -d                           # serve, healthy immediately
  ```
- Server loads the index once at startup (lifespan) → **restart required** to pick up a refreshed index; the deploy scripts enrich-then-up does exactly that.
- Idempotent enrich (content-hash cache) means re-deploys re-embed nothing when the corpus is unchanged; the persisted volume keeps the index across restarts.

## Sizing a RAG service that offloads to a hosted LLM
Compute (LLM + embeddings) runs on the provider (OpenAI/etc.), so the container is mostly I/O: parse docs, hold vectors in RAM (tiny for a few dozen docs), FastAPI + a numpy cosine. **1 CPU / 2 GB RAM is ample** for a small internal corpus — the RAM is for the Python/numpy/tokenizer footprint, not the model.

## docker compose resource caps (single host, no swarm)
`deploy.resources.limits.{cpus,memory}` IS honored by `docker compose up` (v2) — it maps to `--cpus`/`--memory`. `reservations.cpus` is swarm-only (drop it to avoid warnings). Validate with `docker compose config`.

Related: [[Anthropic has no first-party embeddings endpoint]]

%% ai-graph-start %%

**Related notes:**
- [[Local RAG stack for a 1 vCPU 2 GB box e5 + bge-reranker + Qwen 0.5B]]
- [[Make a RAG pipeline testable offline with a deterministic hash embedder]]
- [[docs-vector-search OOMs on ask on a 1vCPU2GB box (Qwen KV cache over RAM+swap)]]
- [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]
- [[Two-phase RAG chatbot UX fast retrieval first, slow generation second]]

%% ai-graph-end %%