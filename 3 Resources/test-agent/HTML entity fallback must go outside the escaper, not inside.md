---
title: "HTML entity fallback must go outside the escaper, not inside"
created: 2026-09-21
type: lesson
status: seedling
source: "session 2026-09-21 run-7bcfe335"
tags: [test-agent, html, reporting, gotcha, escaping]
---

# HTML entity fallback must go outside the escaper, not inside

GOTCHA in the test-agent-v2 HTML report renderers (common/report/*, common/testplan/report/html.py): the em-dash fallback idiom was written `e(x or "&mdash;")`, which escapes the HTML entity into the literal text "&amp;mdash;" whenever x is empty — the user sees "&mdash;" as raw text, not an — dash. Correct idiom: escape the value, THEN fall back to the entity: `e(x) or "&mdash;"`. General rule: never pass an HTML entity THROUGH the escaper; apply the fallback entity AFTER escaping. Same trap for any `e(value or "&nbsp;"/"&hellip;")`.
