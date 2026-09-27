---
ai_hash: c5188e0abeb81a38
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: session 2026-09-09 SCRUM-92
status: seedling
tags:
- claude-code
- artifacts
- svg
- css
- gotcha
title: Embed generated SVG in an artifact via <img> data-URI to isolate its styles
type: lesson
---

# Embed generated SVG in an artifact via <img> data-URI to isolate its styles

When placing a generated SVG (or any third-party HTML fragment) into a Claude artifact page, embed it as `<img src="data:image/svg+xml;base64,...">`, NOT as inline `<svg>` markup.

**Why:** CSS inside an inline `<svg><style>...</style>` is NOT scoped to the SVG — bare element selectors (`text {}`) and class selectors (`.legend {}`, `.title {}`) apply to the whole document and silently override the page. An `<img>` renders the SVG as an isolated replaced element, so its internal styles cannot leak. Vector scaling and crispness are unaffected.

**Gotcha within the gotcha:** do NOT try to scope inline SVG CSS by naively splitting the stylesheet on `}` and prefixing each selector — that corrupts any nested at-rules (`@media`, `@keyframes`) because the split breaks their braces, leaving the page CSS unbalanced. If you must inline, give the svg an id and prefix selectors with a proper CSS parser, or just use the `<img>` data-URI and avoid the problem.

Surfaced building the SCRUM-92 sprint-dashboard artifact (2026-09-09), embedding a fireworks-tech-graph diagram.

## Related

- [[Live artifacts need a republish loop]]
- [[not client-side fetch]]

%% ai-graph-start %%

**Related notes:**
- [[Animate fireworks SVGs via injected CSS keyframes; Style-12 Ops Pulse constraints]]
- [[Embed a fireworks-tech-graph SVG in an HTML artifact and animate it with CSS via its data-flowid hooks]]
- [[Artifacts render mermaid natively — never add a mermaid CDN script (CSP blocks it)]]
- [[Embed brand SVG icons in an Excalidraw diagram (image element + files dataURL; Simple Icons CDN)]]
- [[fireworks-tech-graph skill JSON-IR render pipeline and quality_profile gotcha]]

%% ai-graph-end %%