---
title: "Cap-before-exclude parallelism recall trap"
created: 2026-09-22
type: lesson
status: seedling
source: "test-agent-v2 parallel-gather, 2026-09-22"
tags: [concurrency, parallelism, recall, gotcha, test-agent]
---

# Cap-before-exclude parallelism recall trap

When a source produces a list and applies a **top-N cap _after_ filtering an exclude/seen set**, you cannot naively parallelize sibling sources. In the serial design each source sees the growing exclude (everything prior sources found) and spends its N cap slots on items *distinct from the union*. Run them in parallel and each sees only the **initial** exclude, so two sources can both spend a slot on the same item; the post-fan-out merge-dedup then drops the overlap, and the union nets **fewer unique results than serial** — a silent recall loss that hides behind a "no behaviour change" refactor label.

**When it bites:** any fan-out of capped, exclude-aware producers (search top-K, ranked top-N) whose id-spaces overlap.

**Fixes (cheapest first):**
- Run the free/instant producer first and fold its output into the shared exclude the parallel wave sees (recovers part of the forward-dedup for free).
- Keep near-disjoint id-spaces uncoordinated (overlap is then negligible).
- If measurement shows a real drop: **post-dedup cap re-fill** — let producers return more than N, apply the per-source cap after the global merge.

Concrete case: test-agent-v2 KGA `expansion_round` — every seed producer filters `exclude` before its cap (all `max_seeds=5`, cloud top-8). A deterministic A/B (`tools/gather_fanout_ab.py`) confirmed the ceiling **= the cross-source overlap count** (worst case: serial 12 unique → parallel 11, losing the one id that `atlassian_search` and `ground_leads` both surfaced). The overlapping pair is `atlassian_search` × `ground_leads` — they **share** Jira/Confluence id-space; `cloud_discover` is disjoint (so my first "near-disjoint" hand-wave was wrong — check which producers actually share an id-space). Also confirmed: the seed set is **invariant to the concurrency level** (`set(N=1) == set(N=4)`) — the loss is inherent to parallel-merge-vs-serial-forward-exclude at any N, not a race.

Related: [[Shared model quota makes LLM fan-out worthless]]

## Related

- [[Shared model quota makes LLM fan-out worthless]]
