---
title: "OpenAI request gotchas: 8192-token embedding limit and max_completion_tokens"
created: 2026-09-05
type: gotcha
status: seedling
source: "session 2026-09-05 (docs-vector-search CI enrich 400s)"
tags: [openai, embeddings, tiktoken, rag, gotcha]
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
