---
ai_hash: 0c0b92782ab695b0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities: []
source: session 2026-09-21
status: seedling
tags:
- css
- gotcha
- artifact
- epost
- regex
title: Naive @import strip regex ate the :root block (unstyled artifact)
type: lesson
---

# Naive @import strip regex ate the :root block (unstyled artifact)

GOTCHA (ePost/HTML artifact rendering, 2026-09-21): the ePost artifacts rendered COMPLETELY UNSTYLED (no yellow hero, no cards, default font) even though the <style> block was present and the base64 @font-face were well-formed. Root cause: when inlining the design-system colors_and_type.css I stripped its Google-Fonts @import with a naive regex `@import[^;]*;`. But the URL contains an internal semicolon — `family=JetBrains+Mono:wght@400;500` — so the regex stopped at the FIRST `;`, truncating mid-URL and leaving orphan junk `500&display=swap');` right before `:root {`. That stray single-quote OPENS a CSS string that runs to the next quote (`--font-sans: 'SwissPostSans'`), swallowing the ENTIRE :root custom-property block as one malformed string token. Result: no `--brand`/`--fg`/etc. defined, every `var(--…)` empty → whole page unstyled. FIX: strip the whole statement incl. internal `;` with `re.sub(r"@import\s+url\([^)]*\)\s*;", "", css)`. LESSONS: (1) never strip a CSS @import/url with `[^;]*` — URLs legally contain `;`; match the `url(...)` parens. (2) A single stray quote in inlined CSS can silently eat a large region (string token runs to the next quote) — symptom is a totally-unstyled page, not a local glitch. (3) Verify inlined CSS by asserting `:root {` + a known token (`--brand:`) survive and no orphan fragments remain. Also: drop CDN <script> loaders in artifacts (CSP blocks external hosts); the Artifact viewer renders <pre class="mermaid"> natively.

%% ai-graph-start %%

**Related notes:**
- [[Render a test-agent run in ePost design client-side from the bank]]
- [[Inline SVG ignores theme unless shapes use CSS-variable classes, not hardcoded hex]]
- [[Embed generated SVG in an artifact via img data-URI to isolate its styles]]

%% ai-graph-end %%