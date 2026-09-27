---
ai_hash: 93d5dd95d5c8881c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities:
- HTML report artifacts
- test-plan
- knowledge-preview
- plan-preview
- QA/QC final report
- scenarios
- per-step detail
- run-8eafe7ea
- summary-first format
- collapsible details
- artifact output
- chat length
- common/testplan/report/html.py
- common/report/knowledge.py
- common/report/plan_preview.py
source: session 2026-09-21
status: seedling
tags:
- feedback
- test-agent
- reporting
- concise
title: HTML report artifacts should be more concise
type: lesson
---

# HTML report artifacts should be more concise

User feedback: the RENDERED HTML report artifacts (test-plan / knowledge-preview / plan-preview) are too long and too detailed. The QA/QC final report especially — it renders all scenarios with full per-step detail (run-8eafe7ea = ~490KB). Prefer a more concise deliverable: summary-first, details collapsible or omitted. Applies to the artifact output, NOT chat length. Renderers live in common/testplan/report/html.py, common/report/knowledge.py, common/report/plan_preview.py.

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 retired agent-side HTML renderers — agent is data-only, client renders]]
- [[test-agent-v2 always-enriched HTML report generator]]
- [[TPD methodology list had dupes + substring false-positives]]
- [[TPD test_kinds must be additive over the base four, not replace them]]
- [[test-agent-v2 persists diagram-as-code + get_deliverables tool]]

**Relations:**
- HTML report artifacts — *are described as* — too long
- HTML report artifacts — *are described as* — too detailed
- test-plan — *is a type of* — HTML report artifacts
- knowledge-preview — *is a type of* — HTML report artifacts
- plan-preview — *is a type of* — HTML report artifacts
- QA/QC final report — *is an example of* — HTML report artifacts
- QA/QC final report — *renders* — scenarios
- scenarios — *include* — per-step detail
- run-8eafe7ea — *is an instance of* — QA/QC final report
- run-8eafe7ea — *has size* — ~490KB
- HTML report artifacts — *should adopt* — summary-first format
- HTML report artifacts — *should include* — collapsible details
- request applies to — *artifact output* — HTML report artifacts
- request does not apply to — *chat length* — HTML report artifacts
- common/testplan/report/html.py — *renders* — test-plan
- common/report/knowledge.py — *renders* — knowledge-preview
- common/report/plan_preview.py — *renders* — plan-preview

%% ai-graph-end %%