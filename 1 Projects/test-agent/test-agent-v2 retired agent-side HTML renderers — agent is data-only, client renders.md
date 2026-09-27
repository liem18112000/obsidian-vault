---
ai_hash: 4ba8566fd93ddeaf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities:
- test-agent-v2
- agent-side HTML renderers
- agent
- client
- HTML report renderers
- deterministic renderers
- common/report/{util,knowledge,plan_preview}.py
- common/testplan/report/html.py
- tools/build_*_report.py
- format bugs
- HTML-entity fallback
- Confluence storage-format
- ADF
- note.synopsis
- claude.ai Artifact sandbox iframe
- data-URI downloads
- New architecture
- full run data
- get_* tools
- get_understanding
- get_questions
- get_plan
- decisions
- get_scenarios
- steps
- test-data
- get_coverage
- benchmark_run
- local Claude Code
- rendering skill
- user
- HTML artifact
- Claude Doc
- Markdown
- artifact kind
- section specs
- stored prompts
- bridge/prompts.py steps 2b/3c/4b/7
- '"Rendering artifacts" note'
- testplan/llm/templates.py tpd.report
- kga planners/templates.py kga.report
- deterministic server-side renderer
- rich, messy source data
- LLM client renderer
- variability
- assemble_plan methodology dedupe
- data-correctness
- per-run artifact registry
- record_artifact
- get_run
- client-published URLs
- Commit adf1521
- LOC
source: session 2026-09-21
status: seedling
tags:
- test-agent
- architecture
- decision
- reporting
- rendering
title: test-agent-v2 retired agent-side HTML renderers — agent is data-only, client
  renders
type: decision
---

# test-agent-v2 retired agent-side HTML renderers — agent is data-only, client renders

DECISION (test-agent-v2, 2026-09-21): retire ALL agent-side HTML report renderers. The deterministic renderers (common/report/{util,knowledge,plan_preview}.py, common/testplan/report/html.py, tools/build_*_report.py) were brittle — a stream of format bugs (HTML-entity fallback escaped into literal "&amp;mdash;"; raw Confluence storage-format / ADF leaking into source blurbs because note.synopsis is stored verbatim; duplicated methodology; and the claude.ai Artifact sandbox iframe silently blocking data-URI downloads). 

New architecture: the AGENT is data-only — it stores the full run and returns it via the get_* tools (get_understanding, get_questions, get_plan + decisions, get_scenarios + steps + test-data, get_coverage, benchmark_run). The CLIENT (local Claude Code) renders each artifact (knowledge review / plan review / final test plan) using an appropriate rendering skill; if it has NO suitable skill, it ASKS THE USER how they want it presented (HTML artifact / Claude Doc / Markdown). Whatever the format, it must carry the FULL info for that artifact kind — the section specs that used to live in the renderer now live in the stored prompts (bridge/prompts.py steps 2b/3c/4b/7 + "Rendering artifacts" note; testplan/llm/templates.py tpd.report; kga planners/templates.py kga.report).

RATIONALE: a deterministic server-side renderer of rich, messy source data will always chase format bugs; an LLM client renderer handles the variability and can adapt format to the user. Kept: the assemble_plan methodology dedupe (data-correctness, not rendering) and the per-run artifact registry (record_artifact/get_run) so client-published URLs stay retrievable. Commit adf1521 (net -1567 LOC).

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 always-enriched HTML report generator]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
- [[test-agent-v2 persists diagram-as-code + get_deliverables tool]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[HTML report artifacts should be more concise]]

**Relations:**
- test-agent-v2 — *retired* — agent-side HTML renderers
- agent — *is* — data-only
- client — *renders* — HTML
- agent-side HTML renderers — *are a type of* — HTML report renderers
- HTML report renderers — *were* — brittle
- deterministic renderers — *are a type of* — HTML report renderers
- deterministic renderers — *include* — common/report/{util,knowledge,plan_preview}.py
- deterministic renderers — *include* — common/testplan/report/html.py
- deterministic renderers — *include* — tools/build_*_report.py
- HTML-entity fallback — *caused* — format bugs
- Confluence storage-format — *caused* — format bugs
- ADF — *caused* — format bugs
- note.synopsis — *stores* — Confluence storage-format
- note.synopsis — *stores* — ADF
- claude.ai Artifact sandbox iframe — *blocked* — data-URI downloads
- New architecture — *defines* — agent is data-only
- agent — *stores* — full run data
- agent — *returns* — full run data
- full run data — *returned via* — get_* tools
- get_* tools — *include* — get_understanding
- get_* tools — *include* — get_questions
- get_* tools — *include* — get_plan
- get_* tools — *include* — decisions
- get_* tools — *include* — get_scenarios
- get_* tools — *include* — steps
- get_* tools — *include* — test-data
- get_* tools — *include* — get_coverage
- get_* tools — *include* — benchmark_run
- client — *is* — local Claude Code
- client — *renders* — each artifact
- client — *uses* — rendering skill
- client — *asks* — user
- user — *can choose* — HTML artifact
- user — *can choose* — Claude Doc
- user — *can choose* — Markdown
- artifact kind — *must carry* — full info
- section specs — *used to live in* — renderer
- section specs — *now live in* — stored prompts
- stored prompts — *include* — bridge/prompts.py steps 2b/3c/4b/7
- stored prompts — *include* — "Rendering artifacts" note
- stored prompts — *include* — testplan/llm/templates.py tpd.report
- stored prompts — *include* — kga planners/templates.py kga.report
- deterministic server-side renderer — *chases* — format bugs
- deterministic server-side renderer — *processes* — rich, messy source data
- LLM client renderer — *handles* — variability
- LLM client renderer — *adapts format to* — user
- assemble_plan methodology dedupe — *is* — kept
- assemble_plan methodology dedupe — *ensures* — data-correctness
- per-run artifact registry — *is* — kept
- per-run artifact registry — *includes* — record_artifact
- per-run artifact registry — *includes* — get_run
- per-run artifact registry — *ensures* — client-published URLs stay retrievable
- Commit adf1521 — *resulted in* — -1567 LOC

%% ai-graph-end %%