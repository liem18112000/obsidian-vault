---
title: "Embed generated SVG in an artifact via <img> data-URI to isolate its styles"
created: 2026-09-09
type: lesson
status: seedling
source: "session 2026-09-09 SCRUM-92"
tags: [claude-code, artifacts, svg, css, gotcha]
---

# Embed generated SVG in an artifact via <img> data-URI to isolate its styles

When placing a generated SVG (or any third-party HTML fragment) into a Claude artifact page, embed it as `<img src="data:image/svg+xml;base64,...">`, NOT as inline `<svg>` markup.

**Why:** CSS inside an inline `<svg><style>...</style>` is NOT scoped to the SVG — bare element selectors (`text {}`) and class selectors (`.legend {}`, `.title {}`) apply to the whole document and silently override the page. An `<img>` renders the SVG as an isolated replaced element, so its internal styles cannot leak. Vector scaling and crispness are unaffected.

**Gotcha within the gotcha:** do NOT try to scope inline SVG CSS by naively splitting the stylesheet on `}` and prefixing each selector — that corrupts any nested at-rules (`@media`, `@keyframes`) because the split breaks their braces, leaving the page CSS unbalanced. If you must inline, give the svg an id and prefix selectors with a proper CSS parser, or just use the `<img>` data-URI and avoid the problem.

Surfaced building the SCRUM-92 sprint-dashboard artifact (2026-09-09), embedding a fireworks-tech-graph diagram.

## Related

- [[Live artifacts need a republish loop]]
- [[not client-side fetch]]
