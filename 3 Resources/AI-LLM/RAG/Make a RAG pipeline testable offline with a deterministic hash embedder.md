---
title: "Make a RAG pipeline testable offline with a deterministic hash embedder"
created: 2026-09-05
type: technique
status: seedling
source: "session 2026-09-05 (leo-customer360 docs-vector-search build)"
tags: [rag, testing, ci, embeddings, fastapi, offline]
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
