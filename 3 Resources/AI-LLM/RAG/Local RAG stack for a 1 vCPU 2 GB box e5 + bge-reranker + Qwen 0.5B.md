---
title: "Local RAG stack for a 1 vCPU / 2 GB box: e5 + bge-reranker + Qwen 0.5B"
created: 2026-09-06
type: technique
status: seedling
source: "session 2026-09-06 (local-model RAG plan for leo-customer360)"
tags: [rag, local-models, fastembed, llama-cpp, reranker, embeddings, sizing]
---

# Local RAG stack for a 1 vCPU / 2 GB box: e5 + bge-reranker + Qwen 0.5B

A fully-local RAG stack that fits a **1 vCPU / 2 GB** VM, by component (all in-process, ONNX/GGUF):

## Picks
- **Embed:** `intfloat/multilingual-e5-small` (~470MB, use `query:`/`passage:` prefixes) for multilingual corpora (e.g. Vietnamese); `BAAI/bge-small-en-v1.5` (~130MB) for English-only. Run via **fastembed** (`TextEmbedding`). 384-dim.
- **Rerank:** `BAAI/bge-reranker-base` (~280MB) via fastembed `TextCrossEncoder` — cross-encode the top-20 retrieved chunks down to top-3-5. +100-300ms on CPU. (`bge-reranker-v2-m3` is better for VN but ~600MB — too big here.)
- **Generate:** `Qwen2.5-0.5B-Instruct` Q4_K_M GGUF (~400-600MB) via `llama-cpp-python`. Apache-2.0.
- **Vector store:** `sqlite-vec` (single file, persists, SQL filter) or FAISS flat — skip a DB server; a few MB for thousands of chunks.

## The load-bearing insight
On a tiny generator, **retrieval + reranking quality matters MORE than the LLM**. A 0.5B model hallucinates unless you feed it tight, correct context — so chunk the docs, retrieve top-20, rerank to top-5, and prompt strictly ("answer only from the context; say you dont know otherwise"). Spend the effort on the retriever, not the generator.

## RAM tiering (the 2 GB is the constraint)
Resident model RAM: e5 (~500MB) + reranker (~300MB) + Qwen 0.5B (~600MB) ≈ **1.4GB** — plus ~300MB for Python/onnxruntime/FastAPI. That is TIGHT on 2GB.
- **Hybrid (safe default):** keep embed + rerank local, **offload generation to a hosted API** (OpenAI / GreenNode MaaS — OpenAI-compatible) → ~0.8GB resident.
- **Full local:** add Qwen; needs a **swapfile**. If it OOMs, shed in this order: reranker first, then generation to hosted.

## Design consequence
Keep two provider seams — `embed()` and `chat()` — plus a rerank stage, all env-selected, so hybrid vs full-local is a **config flip, not a code change**. Chunking is required (whole-doc vectors defeat reranking).

Related: [[Anthropic has no first-party embeddings endpoint]], [[Build a RAG index as an explicit deploy step, run a serve-only container]]
