---
title: "OpenAIEmbeddings needs check_embedding_ctx_length=False against Ollama"
created: 2026-09-10
type: lesson
status: seedling
source: "session 2026-09-10 — local RAGAS judge"
tags: [ragas, ollama, langchain, embeddings, gotcha]
---

# OpenAIEmbeddings needs check_embedding_ctx_length=False against Ollama

LangChain `OpenAIEmbeddings` pointed at an Ollama OpenAI-compatible endpoint (`base_url=http://localhost:11434/v1`) fails with **HTTP 400 `invalid input type`** unless you pass **`check_embedding_ctx_length=False`**.

**Why:** by default `OpenAIEmbeddings` client-side tokenizes each input with tiktoken and sends **arrays of integer token IDs** to `/v1/embeddings` (an optimization the real OpenAI API accepts). Ollama's embeddings endpoint only accepts **strings**, so the int arrays are rejected. Setting `check_embedding_ctx_length=False` disables that tokenization path so raw strings are sent.

```python
OpenAIEmbeddings(model="nomic-embed-text", api_key="ollama",
                 base_url="http://localhost:11434/v1",
                 check_embedding_ctx_length=False)   # <- required for Ollama
```

Comes up when using a **local judge for RAGAS** (or any langchain embeddings) via Ollama instead of a hosted key. Same trap applies to other OpenAI-compatible servers that do not implement token-array inputs (vLLM in some configs, LM Studio). The chat/completions side has no equivalent issue — only embeddings.

Related: [[Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk]]

## Related

- [[Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk]]
