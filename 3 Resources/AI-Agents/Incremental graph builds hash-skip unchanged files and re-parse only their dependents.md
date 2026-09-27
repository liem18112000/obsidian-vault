---
ai_hash: b4c9512811733cea
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Incremental graph builds
- Code graph
- Hash-based skip
- SHA-256
- Dependent tracing
- git diff
- import edges
- stale edges
- embeddings
- Vertex AI
- adjacency lists
- BFS
- incremental pipeline
- invalidation set
- changed inputs
- A code knowledge graph
source: 'Confluence: Code Knowledge Graph - Apply in AI Test Driven (2026-03-25)'
status: seedling
tags:
- code-knowledge-graph
- incremental-build
- caching
- static-analysis
- gotcha
title: Incremental graph builds hash-skip unchanged files and re-parse only their
  dependents
type: lesson
---

# Incremental graph builds hash-skip unchanged files and re-parse only their dependents

Re-parsing an entire repository on every change makes a code graph too slow to keep current, and a stale graph is worse than none. The incremental build uses two mechanisms together:

1. **Hash-based skip** — each file's **SHA-256** is stored with its nodes. On rebuild, files whose hash is unchanged are not re-parsed at all.
2. **Dependent tracing** — `git diff` gives the changed files, then **import edges are walked backwards** to find files that depend on them, and those are re-parsed too.

Step 2 is the one that is easy to omit and the reason naive incremental builds go subtly wrong. A file whose own bytes did not change can still have **stale edges**: if `b.py` deleted the function `a.py` was calling, `a.py`'s `CALLS` edge now points at nothing, and only re-parsing `a.py` discovers it. Hashing alone gives a graph that is individually correct per file and globally wrong.

The same idea applies to the derived data: **embeddings** are regenerated with their own hash-based skip and batched into single Vertex AI calls, and in-memory **adjacency lists** for BFS are cached and invalidated on write.

General principle worth lifting out: **in any incremental pipeline, the invalidation set is the changed inputs plus everything that referenced them.** Getting the first half right is easy and feels finished, which is precisely why the second half gets skipped.

## Related

- [[A code knowledge graph answers impact questions that embedding search cannot]]

## Related

- [[A code knowledge graph answers impact questions that embedding search cannot]]

%% ai-graph-start %%

**Related notes:**
- [[A code knowledge graph answers impact questions that embedding search cannot]]
- [[Code Knowledge Graph - Apply in AI Test Driven]]

**Relations:**
- Incremental graph builds — *improves* — Code graph
- Incremental graph builds — *employs* — Hash-based skip
- Incremental graph builds — *employs* — Dependent tracing
- Hash-based skip — *utilizes* — SHA-256
- Dependent tracing — *uses* — git diff
- Dependent tracing — *processes* — import edges
- naive incremental builds — *can lead to* — stale edges
- embeddings — *regenerated with* — Hash-based skip
- embeddings — *batched into* — Vertex AI
- adjacency lists — *support* — BFS
- adjacency lists — *are* — cached
- adjacency lists — *invalidated by* — write
- incremental pipeline — *defines* — invalidation set
- invalidation set — *comprises* — changed inputs
- invalidation set — *comprises* — everything that referenced them
- Incremental graph builds — *is related to* — A code knowledge graph

%% ai-graph-end %%