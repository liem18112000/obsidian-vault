---
ai_hash: f66da8fc49ab54ca
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-01
entities: []
source: session 2026-09-01 — Testing Agent codegraph
status: seedling
tags:
- graphify
- code-graph
- tree-sitter
- networkx
- gotcha
title: graphify graph.json is NetworkX node-link JSON — edges are under "links", not
  "edges"
type: gotcha
---

# graphify graph.json is NetworkX node-link JSON — edges are under "links", not "edges"

graphify (the tree-sitter code-intelligence CLI, PyPI `graphifyy`) writes its graph to `graphify-out/graph.json` in **NetworkX node-link JSON** format. Top-level keys: `directed, graph, hyperedges, links, multigraph, nodes`. The edge list is under **`links`**, NOT `edges` — code that reads `graph["edges"]` silently gets nothing. Nodes are under `nodes`; each node carries `id`, `label`, `source_file`, and `metadata{kind, language}`.

**Why it matters:** a consumer aggregating the graph (file-level rollup, dependency edges, API-surface scan) must iterate `graph["links"]` for edges and `graph["nodes"]` for symbols. The human-readable summary (node/edge/community counts, god-nodes, surprising connections) comes from the sibling `GRAPH_REPORT.md`, not from graph.json — parse that for counts and read graph.json only for structure.

Verified with graphify 0.9.53 via `graphify update <dir> --force` (100%% local, no API key). Found while building the `codegraph` fetcher in the ai-agentic-framework Testing Agent, whose runner initially used a `graph.get("edges", [])` fallback that always returned 0.

Related: [[graphify code knowledge graph]] [[NetworkX]]

## Related

- [[graphify code knowledge graph]]
- [[NetworkX]]

%% ai-graph-start %%

**Related notes:**
- [[graphify offline workflow extract --code-only then cluster-only]]

%% ai-graph-end %%