---
title: "HTML report artifacts should be more concise"
created: 2026-09-21
type: lesson
status: seedling
source: "session 2026-09-21"
tags: [feedback, test-agent, reporting, concise]
---

# HTML report artifacts should be more concise

User feedback: the RENDERED HTML report artifacts (test-plan / knowledge-preview / plan-preview) are too long and too detailed. The QA/QC final report especially — it renders all scenarios with full per-step detail (run-8eafe7ea = ~490KB). Prefer a more concise deliverable: summary-first, details collapsible or omitted. Applies to the artifact output, NOT chat length. Renderers live in common/testplan/report/html.py, common/report/knowledge.py, common/report/plan_preview.py.
