---
ai_hash: 54dae6b0a9bfb703
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities: []
source: session 2026-09-21 run-7bcfe335
status: seedling
tags:
- test-agent
- html
- reporting
- gotcha
- escaping
title: HTML entity fallback must go outside the escaper, not inside
type: lesson
---

# HTML entity fallback must go outside the escaper, not inside

GOTCHA in the test-agent-v2 HTML report renderers (common/report/*, common/testplan/report/html.py): the em-dash fallback idiom was written `e(x or "&mdash;")`, which escapes the HTML entity into the literal text "&amp;mdash;" whenever x is empty — the user sees "&mdash;" as raw text, not an — dash. Correct idiom: escape the value, THEN fall back to the entity: `e(x) or "&mdash;"`. General rule: never pass an HTML entity THROUGH the escaper; apply the fallback entity AFTER escaping. Same trap for any `e(value or "&nbsp;"/"&hellip;")`.

%% ai-graph-start %%

**Related notes:**
- _(none above threshold)_

%% ai-graph-end %%