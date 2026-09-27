---
title: "Proposal: compare_runs — cross-run consensus/divergence diff for the same ticket"
created: 2026-09-15
type: argument
status: seedling
source: "session 2026-09-15"
tags: [testing-agent, memory, retrieval, proposal, admin-tools, test-agent-v2]
---

# Proposal: compare_runs — cross-run consensus/divergence diff for the same ticket

**Idea (user-proposed, 2026-09-15):** testing a ticket multiple times produces multiple runs; add a memory-retrieval action that reuses prior-run ticket knowledge and shows COMMON vs DIFFERENT points across runs.

**Current state:** knowledge reuse across runs ALREADY happens implicitly — the memory bank is shared, so a new gather_knowledge for the same ticket surfaces 'Prior knowledge from memory (N related nodes)': insights/lessons/scenarios from earlier runs (structural + semantic pgvector recall, grounded by the B4/B5 de-bias). Tools search_memory / search_lessons query it. What's MISSING is a structured run-to-run comparison.

**Proposed `compare_runs(ctx_a, ctx_b)`** (admin/retrieval tool, sibling of list_runs/get_run):
- auto-match runs by the same seed/Jira id (e.g. 'compare the last two LUZ-158230 runs').
- diff per layer: understanding briefs (scope/in-out/decisions), plan (methodology/scope/metrics), scenarios (set diff by kind+title), insights/lessons (statement intersection).
- output two columns: COMMON = in both = robust, model-independent → high confidence, promote/keep; DIFFERENT = only-in-one or changed = model variance OR genuine refinement → low confidence, flag for review.

**Why it fits the memory model:** it's a cross-RUN consistency/consensus score — the same principle as the assured-loop judge (which scores across ROUNDS), lifted to across runs. Feeds de-bias: consensus points earn confidence; divergent ones get surfaced rather than silently averaged into recall. Good for regression/drift detection between re-tests and for building a consensus understanding.

Status: design-only, not built. LUZ-158230 already has 4+ runs (run-1c970d72/85c6948c/c36ec4d5/cd156028) — a ready test case.

Related: [[Refine interrogation caps open questions at 7 per round (max_questions), overflow deferred]]
