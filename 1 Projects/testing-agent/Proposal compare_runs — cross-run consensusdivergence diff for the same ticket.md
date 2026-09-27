---
ai_hash: 60e774a821c1c693
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- Proposal compare_runs
- compare_runs
- ticket
- runs
- memory-retrieval action
- prior-run ticket knowledge
- COMMON points
- DIFFERENT points
- memory bank
- gather_knowledge
- Prior knowledge from memory
- insights
- lessons
- scenarios
- structural recall
- semantic pgvector recall
- B4/B5 de-bias
- search_memory
- search_lessons
- structured run-to-run comparison
- ctx_a
- ctx_b
- admin/retrieval tool
- list_runs
- get_run
- seed/Jira id
- LUZ-158230
- understanding briefs
- plan
- methodology
- scope
- metrics
- kind+title
- statement intersection
- model-independent
- high confidence
- model variance
- genuine refinement
- low confidence
- review
- memory model
- cross-RUN consistency/consensus score
- assured-loop judge
- ROUNDS
- de-bias
- consensus points
- confidence
- divergent ones
- regression/drift detection
- consensus understanding
- design-only
- run-1c970d72
- run-85c6948c
- run-c36ec4d5
- run-cd156028
- Refine interrogation caps open questions at 7 per round (max_questions), overflow
  deferred
source: session 2026-09-15
status: seedling
tags:
- testing-agent
- memory
- retrieval
- proposal
- admin-tools
- test-agent-v2
title: 'Proposal: compare_runs — cross-run consensus/divergence diff for the same
  ticket'
type: argument
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

%% ai-graph-start %%

**Related notes:**
- [[TPD assured-loop judge penalizes cross-run scenario duplication]]
- [[Deployed Testing-Agent refine recommendations are speculative until validated]]
- [[Isolate the same scenarios on both branches to separate regression from flakiness]]
- [[Testing-agent refine flags low confidence when spec PDFs are recorded-only]]
- [[Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance]]

**Relations:**
- Proposal compare_runs — *is a* — Proposal
- compare_runs — *is a* — memory-retrieval action
- compare_runs — *reuses* — prior-run ticket knowledge
- compare_runs — *shows* — COMMON points
- compare_runs — *shows* — DIFFERENT points
- ticket — *produces* — runs
- memory bank — *is* — shared
- gather_knowledge — *surfaces* — Prior knowledge from memory
- Prior knowledge from memory — *includes* — insights
- Prior knowledge from memory — *includes* — lessons
- Prior knowledge from memory — *includes* — scenarios
- Prior knowledge from memory — *uses* — structural recall
- Prior knowledge from memory — *uses* — semantic pgvector recall
- Prior knowledge from memory — *grounded by* — B4/B5 de-bias
- search_memory — *queries* — memory bank
- search_lessons — *queries* — memory bank
- compare_runs — *is a* — structured run-to-run comparison
- compare_runs — *takes arguments* — ctx_a
- compare_runs — *takes arguments* — ctx_b
- compare_runs — *is an* — admin/retrieval tool
- compare_runs — *is a sibling of* — list_runs
- compare_runs — *is a sibling of* — get_run
- compare_runs — *auto-matches* — runs
- runs — *matched by* — seed/Jira id
- compare_runs — *diffs* — understanding briefs
- compare_runs — *diffs* — plan
- compare_runs — *diffs* — scenarios
- compare_runs — *diffs* — insights
- compare_runs — *diffs* — lessons
- COMMON points — *are* — robust
- COMMON points — *are* — model-independent
- COMMON points — *have* — high confidence
- DIFFERENT points — *indicate* — model variance
- DIFFERENT points — *indicate* — genuine refinement
- DIFFERENT points — *have* — low confidence
- DIFFERENT points — *flag for* — review
- compare_runs — *fits* — memory model
- compare_runs — *is a* — cross-RUN consistency/consensus score
- cross-RUN consistency/consensus score — *same principle as* — assured-loop judge
- assured-loop judge — *scores across* — ROUNDS
- compare_runs — *feeds* — de-bias
- consensus points — *earn* — confidence
- divergent ones — *get* — surfaced
- compare_runs — *good for* — regression/drift detection
- compare_runs — *good for* — consensus understanding
- compare_runs — *status* — design-only
- LUZ-158230 — *has* — runs
- LUZ-158230 — *is a* — test case
- LUZ-158230 — *includes* — run-1c970d72
- LUZ-158230 — *includes* — run-85c6948c
- LUZ-158230 — *includes* — run-c36ec4d5
- LUZ-158230 — *includes* — run-cd156028
- run-1c970d72 — *is a* — run
- run-85c6948c — *is a* — run
- run-c36ec4d5 — *is a* — run
- run-cd156028 — *is a* — run
- Proposal compare_runs — *related to* — Refine interrogation caps open questions at 7 per round (max_questions), overflow deferred

%% ai-graph-end %%