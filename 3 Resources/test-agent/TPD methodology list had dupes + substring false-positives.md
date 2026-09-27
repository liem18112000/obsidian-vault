---
ai_hash: 7a6dd65795faf28b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities: []
source: session 2026-09-21 run-7bcfe335
status: seedling
tags:
- test-agent
- tpd
- gotcha
- parsing
title: TPD methodology list had dupes + substring false-positives
type: lesson
---

# TPD methodology list had dupes + substring false-positives

GOTCHA: test-agent-v2 TPD plan assembler (test_plan_definition/define/plan.py assemble_plan) built plan.methodology by `methodology += [m for m in ("api","e2e","ui") if m in d.chosen.lower()]` over every methodology decision. Two bugs: (1) no dedup → one entry per decision → "api, api, api, ui, e2e" in every report that joins plan.methodology; (2) naive substring `m in text` matched "ui" inside words like "build"/"require" and "api" inside "rapidly". Fix: word-boundary regex `re.search(rf"\b{m}\b", ...)` + order-preserving dedup `list(dict.fromkeys(methodology))` (the codebase idiom, cf effective_kinds). Root-cause fix at assemble; also normalize defensively at each report build since older runs persisted the dupes.

%% ai-graph-start %%

**Related notes:**
- [[TPD test_kinds must be additive over the base four, not replace them]]
- [[TPD assured-loop judge penalizes cross-run scenario duplication]]
- [[Fix TPD scenario generator truncation — raise max_tokens, keep one call]]
- [[test-agent-v2 test_evaluation restructure engine + config packages + merged golden set]]
- [[test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate]]

%% ai-graph-end %%