---
title: "Anthropic has no first-party embeddings endpoint"
created: 2026-09-05
type: lesson
status: seedling
source: "session 2026-09-05 (leo-customer360 docs vector-search plan)"
tags: [claude, anthropic, rag, embeddings, vector-search, gotcha]
---

# Anthropic has no first-party embeddings endpoint

RAG or vector-search built to run **on Claude** is a two-provider system: Claude (the official \`anthropic\` SDK) synthesizes answers and does structured extraction, but **Anthropic exposes no embeddings endpoint of its own** — the vectors must come from a separate provider.

## Why it matters
It is easy to assume "use Claude for everything" and then discover there is no `client.embeddings.create`. Plan for a second provider from the start.

## What to use for embeddings
- **Voyage AI** — `voyage-3` / `voyage-3-large`. Anthropic-aligned; simple API key. Good default.
- **Google Vertex** — `text-embedding-005` (native `:predict`).
- **Local** — `nomic-embed-text` (Ollama) or `sentence-transformers` for zero external calls.

## Design rule — two seams
Keep `chat()` (Claude) and `embed()` (embeddings provider) as **separate, swappable seams**. Everything between them (cosine ranking, graph expansion, context budgeting) is plain code over your own data. Do **not** reach for an OpenAI-compatible shim to unify them.

## Consistency rule
Documents and the query **must be embedded by the same model + version**, and the embeddings cache key must include the embedding model id — swapping the embedder invalidates every stored vector.

## Claude side (for the synthesis half)
Default `claude-opus-5`; structured extraction via `output_config.format` (or `messages.parse()`); stream long answers. On GCP, swap `Anthropic()` for `AnthropicVertex(project_id, region)` with no other code change.

Related: [[claude-api]]

## Related

- [[claude-api]]
