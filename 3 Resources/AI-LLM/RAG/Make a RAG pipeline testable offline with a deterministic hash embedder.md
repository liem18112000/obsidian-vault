---
ai_hash: 6857656a9f113b68
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities:
- RAG pipeline
- deterministic hash embedder
- embeddings API key
- local development
- CI
- verification
- embed() seam
- real providers
- enrich stage
- index stage
- cosine retrieve stage
- serve stage
- external calls
- embedder (concept)
- text (input)
- lowercased tokens
- fixed-dimension vector
- hash(token) % dim (method)
- L2-normalization
- Bag-of-hashed-tokens (model)
- environment switch
- EMBED_PROVIDER (variable)
- _hash (function)
- hashlib.md5 (module)
- math.sqrt (function)
- plumbing (architecture)
- cache
- retriever
- /search (endpoint)
- /health (endpoint)
- FastAPI app
- import errors
- shape bugs
- wiring bugs
- semantic ranking
- lexical bag-of-words (model)
- Keyword-overlapping queries
- paraphrases
- dev/CI stand-in
- FastAPI TestClient
- no-port smoke test
- lifespan (FastAPI)
- startup index-load
- network port
- LLM-answer endpoint
- chat key
- Consistency (principle)
- documents
- queries
- SAME embedder
- Anthropic
- first-party embeddings endpoint
source: session 2026-09-05 (leo-customer360 docs-vector-search build)
status: seedling
tags:
- rag
- testing
- ci
- embeddings
- fastapi
- offline
title: Make a RAG pipeline testable offline with a deterministic hash embedder
type: technique
---

# Make a RAG pipeline testable offline with a deterministic hash embedder

A RAG service normally cannot run without an embeddings API key, which blocks local dev, CI, and verification. Add a **dependency-free deterministic "hash" embedder** behind the same `embed()` seam as the real providers so the whole pipeline (enrich → index → cosine retrieve → serve) runs with **zero external calls**.

## The embedder (~10 lines)
For each text: bucket its lowercased tokens into a fixed-dim vector via `hash(token) % dim`, then L2-normalize. Bag-of-hashed-tokens. Select it with an env switch, e.g. `EMBED_PROVIDER=hash`.

```python
def _hash(texts, task, dim=256):
    out=[]
    for t in texts:
        v=[0.0]*dim
        for tok in t.lower().split():
            v[int(hashlib.md5(tok.encode()).hexdigest(),16) % dim]+=1.0
        n=math.sqrt(sum(x*x for x in v)) or 1.0
        out.append([x/n for x in v])
    return out
```

## What it does and does NOT do
- DOES prove plumbing: enrich writes the cache, retriever ranks, `/search` and `/health` work, the FastAPI app boots. Catches import errors, shape bugs, wiring bugs.
- Does NOT give semantic ranking — it is lexical bag-of-words. Keyword-overlapping queries rank sensibly; paraphrases do not. Never ship it as the real embedder; it is a dev/CI stand-in only.

## Pair it with FastAPI TestClient for a no-port smoke test
`with TestClient(app) as c:` runs the apps **lifespan** (so a startup index-load actually executes) and lets you assert `/health` and `/search` without binding a port or needing a key. Skip the LLM-answer endpoint (that still needs a real chat key).

Consistency still applies: documents and queries must use the SAME embedder, so run enrich and the query under the same `EMBED_PROVIDER`.

Related: [[Anthropic has no first-party embeddings endpoint]]

## Related

- [[Anthropic has no first-party embeddings endpoint]]

%% ai-graph-start %%

**Related notes:**
- [[Build a RAG index as an explicit deploy step, run a serve-only container]]
- [[Anthropic has no first-party embeddings endpoint]]
- [[Local RAG stack for a 1 vCPU 2 GB box e5 + bge-reranker + Qwen 0.5B]]

**Relations:**
- RAG pipeline — *can be made testable offline with* — deterministic hash embedder
- RAG pipeline — *cannot run without* — embeddings API key
- embeddings API key — *blocks* — local development
- embeddings API key — *blocks* — CI
- embeddings API key — *blocks* — verification
- deterministic hash embedder — *is behind* — embed() seam
- embed() seam — *is used by* — real providers
- RAG pipeline — *includes stage* — enrich stage
- RAG pipeline — *includes stage* — index stage
- RAG pipeline — *includes stage* — cosine retrieve stage
- RAG pipeline — *includes stage* — serve stage
- deterministic hash embedder — *enables* — RAG pipeline
- RAG pipeline — *to run with* — zero external calls
- embedder (concept) — *processes* — text (input)
- embedder (concept) — *uses* — lowercased tokens
- lowercased tokens — *are bucketed into* — fixed-dimension vector
- fixed-dimension vector — *via* — hash(token) % dim (method)
- fixed-dimension vector — *is processed by* — L2-normalization
- deterministic hash embedder — *is a type of* — Bag-of-hashed-tokens (model)
- deterministic hash embedder — *is selected with* — environment switch
- environment switch — *is* — EMBED_PROVIDER (variable)
- _hash (function) — *uses* — hashlib.md5 (module)
- _hash (function) — *uses* — math.sqrt (function)
- deterministic hash embedder — *proves* — plumbing (architecture)
- enrich stage — *writes to* — cache
- retriever — *ranks* — 
- /search (endpoint) — *is functional* — 
- /health (endpoint) — *is functional* — 
- FastAPI app — *boots* — 
- deterministic hash embedder — *catches* — import errors
- deterministic hash embedder — *catches* — shape bugs
- deterministic hash embedder — *catches* — wiring bugs
- deterministic hash embedder — *does not provide* — semantic ranking
- deterministic hash embedder — *is a type of* — lexical bag-of-words (model)
- Keyword-overlapping queries — *rank sensibly with* — deterministic hash embedder
- paraphrases — *do not rank sensibly with* — deterministic hash embedder
- deterministic hash embedder — *is a* — dev/CI stand-in
- FastAPI TestClient — *is used for* — no-port smoke test
- FastAPI TestClient — *runs* — lifespan (FastAPI)
- lifespan (FastAPI) — *executes* — startup index-load
- FastAPI TestClient — *allows asserting* — /health (endpoint)
- FastAPI TestClient — *allows asserting* — /search (endpoint)
- FastAPI TestClient — *does not bind* — network port
- FastAPI TestClient — *does not need* — embeddings API key
- LLM-answer endpoint — *needs* — chat key
- Consistency (principle) — *applies to* — documents
- Consistency (principle) — *applies to* — queries
- documents — *must use* — SAME embedder
- queries — *must use* — SAME embedder
- Anthropic — *has no* — first-party embeddings endpoint
- deterministic hash embedder — *is related to* — Anthropic has no first-party embeddings endpoint

%% ai-graph-end %%