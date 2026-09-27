---
title: "Incremental graph builds hash-skip unchanged files and re-parse only their dependents"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Code Knowledge Graph - Apply in AI Test Driven (2026-03-25)"
tags: [code-knowledge-graph, incremental-build, caching, static-analysis, gotcha]
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
