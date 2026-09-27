---
ai_hash: 7de10f747b2a28bb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities:
- OpenAI request gotchas
- 8192-token embedding limit
- max_completion_tokens
- text-embedding-3-small
- text-embedding-3-large
- '400 Invalid input[N]: maximum input length is 8192 tokens'
- tiktoken
- cl100k_base
- truncate function
- max_tokens
- Chat Completions
- newer models
- o-series
- gpt-5.x
- gpt-4.x
- OpenAI server
- CI
- Anthropic
- first-party embeddings endpoint
- SDKs
- custom CI secret name
- fixed env var
source: session 2026-09-05 (docs-vector-search CI enrich 400s)
status: seedling
tags:
- openai
- embeddings
- tiktoken
- rag
- gotcha
title: 'OpenAI request gotchas: 8192-token embedding limit and max_completion_tokens'
type: gotcha
---

# OpenAI request gotchas: 8192-token embedding limit and max_completion_tokens

Two OpenAI request gotchas that surface only against the real API (they pass local wiring tests):

## 1. Embedding inputs cap at 8192 tokens
`text-embedding-3-small`/`-large` reject any single input over **8192 tokens** with `400 Invalid input[N]: maximum input length is 8192 tokens`. Long documents (research papers, big READMEs) blow this. Fix: truncate each input before embedding, using the models real tokenizer for an exact cut:
```python
import tiktoken
enc = tiktoken.get_encoding("cl100k_base")   # the text-embedding-3 tokenizer
def truncate(text, limit=8000):              # 8000 leaves headroom under 8192
    toks = enc.encode(text)
    return enc.decode(toks[:limit]) if len(toks) > limit else text
```
tiktoken is the CORRECT tokenizer for OpenAI (it undercounts nothing here — unlike using it for Claude, where it is wrong). A char-based cap is unsafe because chars/token varies (code, non-English like Vietnamese).

## 2. Use max_completion_tokens, not max_tokens
`max_tokens` is deprecated in Chat Completions and is **rejected by newer models** (o-series, gpt-5.x). `max_completion_tokens` works across gpt-4.x AND the newer models, so always send that one. A default like `gpt-4.1` hides this; a run against a newer configured model (e.g. from a CI variable) exposes it.

## Why they hid until CI
Both are 400s from the OpenAI server, so code that imports fine and passes a stubbed/offline test still fails on the first real call. Run the real path in CI (with the key) to catch them. Bonus resilience: wrap the per-document LLM extraction in try/except so one bad call logs a warning instead of aborting the whole index build.

Related: [[Anthropic has no first-party embeddings endpoint]], [[A custom CI secret name must be passed to an SDK explicitly — SDKs only auto-read their fixed env var]]

## Related

- [[Anthropic has no first-party embeddings endpoint]]

%% ai-graph-start %%

**Related notes:**
- [[OpenAIEmbeddings needs check_embedding_ctx_length=False against Ollama]]
- [[LLM-as-reranker JSON truncation budget max_tokens for pretty-printed output, not just element count]]
- [[Enabled thinking shares the max_tokens budget and can truncate output]]
- [[A custom CI secret name must be passed to an SDK explicitly — SDKs only auto-read their fixed env var]]
- [[Pass LLM prompts to spawned CLIs via stdin - Windows argv caps at 32K (ENAMETOOLONG)]]

**Relations:**
- OpenAI request gotchas — *include* — 8192-token embedding limit
- OpenAI request gotchas — *include* — max_completion_tokens
- 8192-token embedding limit — *applies to* — text-embedding-3-small
- 8192-token embedding limit — *applies to* — text-embedding-3-large
- 8192-token embedding limit — *causes* — 400 Invalid input[N]: maximum input length is 8192 tokens
- tiktoken — *is* — tokenizer for OpenAI
- tiktoken — *provides* — cl100k_base
- truncate function — *uses* — tiktoken
- truncate function — *mitigates* — 8192-token embedding limit
- max_tokens — *is deprecated in* — Chat Completions
- max_tokens — *is rejected by* — newer models
- newer models — *include* — o-series
- newer models — *include* — gpt-5.x
- max_completion_tokens — *works with* — gpt-4.x
- max_completion_tokens — *works with* — newer models
- OpenAI server — *returns* — 400s
- CI — *catches* — 400s
- Anthropic — *lacks* — first-party embeddings endpoint
- SDKs — *auto-read* — fixed env var
- custom CI secret name — *requires explicit passing to* — SDKs

%% ai-graph-end %%