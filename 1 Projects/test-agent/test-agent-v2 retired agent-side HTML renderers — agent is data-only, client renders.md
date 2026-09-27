---
title: "test-agent-v2 retired agent-side HTML renderers — agent is data-only, client renders"
created: 2026-09-21
type: decision
status: seedling
source: "session 2026-09-21"
tags: [test-agent, architecture, decision, reporting, rendering]
---

# test-agent-v2 retired agent-side HTML renderers — agent is data-only, client renders

DECISION (test-agent-v2, 2026-09-21): retire ALL agent-side HTML report renderers. The deterministic renderers (common/report/{util,knowledge,plan_preview}.py, common/testplan/report/html.py, tools/build_*_report.py) were brittle — a stream of format bugs (HTML-entity fallback escaped into literal "&amp;mdash;"; raw Confluence storage-format / ADF leaking into source blurbs because note.synopsis is stored verbatim; duplicated methodology; and the claude.ai Artifact sandbox iframe silently blocking data-URI downloads). 

New architecture: the AGENT is data-only — it stores the full run and returns it via the get_* tools (get_understanding, get_questions, get_plan + decisions, get_scenarios + steps + test-data, get_coverage, benchmark_run). The CLIENT (local Claude Code) renders each artifact (knowledge review / plan review / final test plan) using an appropriate rendering skill; if it has NO suitable skill, it ASKS THE USER how they want it presented (HTML artifact / Claude Doc / Markdown). Whatever the format, it must carry the FULL info for that artifact kind — the section specs that used to live in the renderer now live in the stored prompts (bridge/prompts.py steps 2b/3c/4b/7 + "Rendering artifacts" note; testplan/llm/templates.py tpd.report; kga planners/templates.py kga.report).

RATIONALE: a deterministic server-side renderer of rich, messy source data will always chase format bugs; an LLM client renderer handles the variability and can adapt format to the user. Kept: the assemble_plan methodology dedupe (data-correctness, not rendering) and the per-run artifact registry (record_artifact/get_run) so client-published URLs stay retrievable. Commit adf1521 (net -1567 LOC).
