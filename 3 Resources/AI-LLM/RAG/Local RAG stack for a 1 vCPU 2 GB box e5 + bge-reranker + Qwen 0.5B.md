---
ai_hash: 756f93fe177d3ae1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-06
entities:
- Local RAG stack
- 1 vCPU 2 GB VM
- e5 model
- bge-reranker model
- Qwen 0.5B model
- intfloat/multilingual-e5-small
- multilingual corpora
- Vietnamese language
- BAAI/bge-small-en-v1.5
- English language
- fastembed
- TextEmbedding
- BAAI/bge-reranker-base
- TextCrossEncoder
- CPU hardware
- bge-reranker-v2-m3
- Qwen2.5-0.5B-Instruct
- Q4_K_M GGUF
- llama-cpp-python
- Apache-2.0
- sqlite-vec
- FAISS flat
- Vector store
- DB server
- retrieval quality
- reranking quality
- LLM
- 0.5B LLM
- context
- retriever
- generator
- Resident model RAM
- Python
- onnxruntime
- FastAPI
- 2GB RAM
- Hybrid configuration
- embedding stage
- reranking stage
- generation stage
- hosted API
- OpenAI
- GreenNode MaaS
- Full local configuration
- swapfile
- embed() provider seam
- chat() provider seam
- rerank stage provider seam
- config flip
- code change
- Chunking
- Anthropic
- first-party embeddings endpoint
- RAG index
- deploy step
- serve-only container
- ONNX
- GGUF
- Python/onnxruntime/FastAPI stack
source: session 2026-09-06 (local-model RAG plan for leo-customer360)
status: seedling
tags:
- rag
- local-models
- fastembed
- llama-cpp
- reranker
- embeddings
- sizing
title: 'Local RAG stack for a 1 vCPU / 2 GB box: e5 + bge-reranker + Qwen 0.5B'
type: technique
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

%% ai-graph-start %%

**Related notes:**
- [[docs-vector-search OOMs on ask on a 1vCPU2GB box (Qwen KV cache over RAM+swap)]]
- [[Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk]]
- [[Local LLM choice for the test-agent workload (Ollama)]]
- [[A 2-4GB local model cannot match Sonnet 5 — plug the real API instead]]
- [[Build a RAG index as an explicit deploy step, run a serve-only container]]

**Relations:**
- Local RAG stack — *is_for* — 1 vCPU 2 GB VM
- Local RAG stack — *includes* — e5 model
- Local RAG stack — *includes* — bge-reranker model
- Local RAG stack — *includes* — Qwen 0.5B model
- Local RAG stack — *fits* — 1 vCPU 2 GB VM
- Local RAG stack — *uses_format* — ONNX
- Local RAG stack — *uses_format* — GGUF
- intfloat/multilingual-e5-small — *is_embedding_model_for* — multilingual corpora
- multilingual corpora — *supports_language* — Vietnamese language
- BAAI/bge-small-en-v1.5 — *is_embedding_model_for* — English language
- intfloat/multilingual-e5-small — *runs_via* — fastembed
- BAAI/bge-small-en-v1.5 — *runs_via* — fastembed
- fastembed — *provides_component* — TextEmbedding
- intfloat/multilingual-e5-small — *has_dimension* — 384
- BAAI/bge-small-en-v1.5 — *has_dimension* — 384
- intfloat/multilingual-e5-small — *has_size_MB* — 470
- BAAI/bge-small-en-v1.5 — *has_size_MB* — 130
- BAAI/bge-reranker-base — *runs_via* — fastembed
- fastembed — *provides_component* — TextCrossEncoder
- BAAI/bge-reranker-base — *has_size_MB* — 280
- BAAI/bge-reranker-base — *adds_latency_on* — CPU hardware
- bge-reranker-v2-m3 — *is_better_for_language* — Vietnamese language
- bge-reranker-v2-m3 — *has_size_MB* — 600
- bge-reranker-v2-m3 — *is_too_big_for* — 1 vCPU 2 GB VM
- Qwen2.5-0.5B-Instruct — *is_format* — Q4_K_M GGUF
- Qwen2.5-0.5B-Instruct — *runs_via* — llama-cpp-python
- Qwen2.5-0.5B-Instruct — *has_size_MB* — 400-600
- Qwen2.5-0.5B-Instruct — *is_licensed_under* — Apache-2.0
- sqlite-vec — *is_type_of* — Vector store
- FAISS flat — *is_type_of* — Vector store
- sqlite-vec — *avoids* — DB server
- FAISS flat — *avoids* — DB server
- retrieval quality — *matters_more_than* — LLM
- reranking quality — *matters_more_than* — LLM
- 0.5B LLM — *requires* — context
- retriever — *focus_effort_on* — generator
- Resident model RAM — *includes_component* — e5 model
- Resident model RAM — *includes_component* — bge-reranker model
- Resident model RAM — *includes_component* — Qwen 0.5B model
- Resident model RAM — *includes_component* — Python
- Resident model RAM — *includes_component* — onnxruntime
- Resident model RAM — *includes_component* — FastAPI
- Resident model RAM — *is_constrained_by* — 2GB RAM
- e5 model — *has_approx_size_MB* — 500
- bge-reranker model — *has_approx_size_MB* — 300
- Qwen 0.5B model — *has_approx_size_MB* — 600
- Python/onnxruntime/FastAPI stack — *has_approx_size_MB* — 300
- Hybrid configuration — *keeps_local* — embedding stage
- Hybrid configuration — *keeps_local* — reranking stage
- Hybrid configuration — *offloads* — generation stage
- generation stage — *offloads_to* — hosted API
- hosted API — *includes_service* — OpenAI
- hosted API — *includes_service* — GreenNode MaaS
- Hybrid configuration — *requires_resident_RAM_MB* — 800
- Full local configuration — *includes_model* — Qwen model
- Full local configuration — *needs* — swapfile
- Full local configuration — *can_shed* — reranking stage
- Full local configuration — *can_shed* — generation stage
- embed() provider seam — *is_provider_seam* — true
- chat() provider seam — *is_provider_seam* — true
- rerank stage provider seam — *is_provider_seam* — true
- config flip — *enables_switching* — Hybrid configuration
- config flip — *enables_switching* — Full local configuration
- config flip — *avoids* — code change
- Chunking — *is_required_for* — reranking
- Anthropic — *has_no* — first-party embeddings endpoint
- RAG index — *is_built_as* — deploy step
- deploy step — *runs_container* — serve-only container

%% ai-graph-end %%