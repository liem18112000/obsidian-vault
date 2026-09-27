---
ai_hash: 786c59f7520b5e01
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities: []
source: session 2026-09-10 — local RAGAS judge
status: seedling
tags:
- ragas
- ollama
- langchain
- embeddings
- gotcha
title: OpenAIEmbeddings needs check_embedding_ctx_length=False against Ollama
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[OpenAI request gotchas 8192-token embedding limit and max_completion_tokens]]
- [[LLM-as-reranker JSON truncation budget max_tokens for pretty-printed output, not just element count]]
- [[RAGAS silently defaults to OpenAI when no llm is injected — pass a provider-sourced judge]]
- [[Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk]]
- [[Reasoning models reject temperature != 1; use litellm.drop_params for multi-model agents]]

%% ai-graph-end %%