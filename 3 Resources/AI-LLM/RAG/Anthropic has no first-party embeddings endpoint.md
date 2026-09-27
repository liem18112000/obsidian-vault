---
ai_hash: 18c86c1122d2ac3b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities:
- Anthropic
- embeddings endpoint
- Claude
- RAG
- vector-search
- two-provider system
- answers
- structured extraction
- '`client.embeddings.create`'
- Voyage AI
- '`voyage-3`'
- '`voyage-3-large`'
- Anthropic-aligned
- Google Vertex
- '`text-embedding-005`'
- Ollama
- '`nomic-embed-text`'
- '`sentence-transformers`'
- Local embeddings
- '`chat()`'
- '`embed()`'
- swappable seams
- OpenAI-compatible shim
- documents
- query
- embedding model id
- embeddings cache key
- stored vector
- '`claude-opus-5`'
- '`output_config.format`'
- '`messages.parse()`'
- '`Anthropic()`'
- '`AnthropicVertex`'
- GCP
- '`project_id`'
- '`region`'
- claude-api
source: session 2026-09-05 (leo-customer360 docs vector-search plan)
status: seedling
tags:
- claude
- anthropic
- rag
- embeddings
- vector-search
- gotcha
title: Anthropic has no first-party embeddings endpoint
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[Claude on Vertex AI uses anthropic[vertex], not google-genai]]
- [[Claude Code runs on Vertex AI via three env vars with gcloud ADC]]
- [[Claude models are available on GCP Vertex AI Model Garden]]
- [[Anthropic has no third-party OAuth; in-app Claude login means driving the claude auth CLI]]
- [[List Anthropic models on Vertex via the publisherModels REST endpoint]]

**Relations:**
- Anthropic — *has no* — embeddings endpoint
- RAG — *built to run on* — Claude
- vector-search — *built to run on* — Claude
- RAG — *is a* — two-provider system
- vector-search — *is a* — two-provider system
- Claude — *synthesizes* — answers
- Claude — *does* — structured extraction
- Anthropic — *exposes no* — embeddings endpoint
- Anthropic — *does not offer* — `client.embeddings.create`
- Voyage AI — *is an embeddings provider* — Anthropic-aligned
- `voyage-3` — *is a model from* — Voyage AI
- `voyage-3-large` — *is a model from* — Voyage AI
- Google Vertex — *is an embeddings provider* — null
- `text-embedding-005` — *is a model from* — Google Vertex
- Ollama — *supports* — `nomic-embed-text`
- `nomic-embed-text` — *is used for* — Local embeddings
- `sentence-transformers` — *is used for* — Local embeddings
- `chat()` — *should be a* — swappable seams
- `embed()` — *should be a* — swappable seams
- `chat()` — *should be separate from* — `embed()`
- OpenAI-compatible shim — *should not unify* — `chat()`
- OpenAI-compatible shim — *should not unify* — `embed()`
- documents — *must be embedded by* — same model
- query — *must be embedded by* — same model
- documents — *must be embedded by* — same version
- query — *must be embedded by* — same version
- embeddings cache key — *must include* — embedding model id
- swapping embedder — *invalidates* — stored vector
- `claude-opus-5` — *is a default model for* — Claude
- structured extraction — *uses* — `output_config.format`
- structured extraction — *uses* — `messages.parse()`
- `Anthropic()` — *is a client for* — Claude
- `AnthropicVertex` — *is a client for* — Claude
- `AnthropicVertex` — *is used on* — GCP
- `AnthropicVertex` — *requires* — `project_id`
- `AnthropicVertex` — *requires* — `region`
- `Anthropic()` — *can be swapped for* — `AnthropicVertex`
- claude-api — *is related to* — Claude

%% ai-graph-end %%