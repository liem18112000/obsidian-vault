---
title: "TPD methodology list had dupes + substring false-positives"
created: 2026-09-21
type: lesson
status: seedling
source: "session 2026-09-21 run-7bcfe335"
tags: [test-agent, tpd, gotcha, parsing]
---

# TPD methodology list had dupes + substring false-positives

GOTCHA: test-agent-v2 TPD plan assembler (test_plan_definition/define/plan.py assemble_plan) built plan.methodology by `methodology += [m for m in ("api","e2e","ui") if m in d.chosen.lower()]` over every methodology decision. Two bugs: (1) no dedup → one entry per decision → "api, api, api, ui, e2e" in every report that joins plan.methodology; (2) naive substring `m in text` matched "ui" inside words like "build"/"require" and "api" inside "rapidly". Fix: word-boundary regex `re.search(rf"\b{m}\b", ...)` + order-preserving dedup `list(dict.fromkeys(methodology))` (the codebase idiom, cf effective_kinds). Root-cause fix at assemble; also normalize defensively at each report build since older runs persisted the dupes.
