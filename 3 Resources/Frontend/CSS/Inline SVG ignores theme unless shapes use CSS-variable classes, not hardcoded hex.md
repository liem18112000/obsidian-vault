---
ai_hash: 597e5f0138d4d32c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-14
entities: []
source: session 2026-07-14 perf report HTML
status: seedling
tags:
- css
- svg
- theming
- dark-mode
- frontend
- gotcha
title: Inline SVG ignores theme unless shapes use CSS-variable classes, not hardcoded
  hex
type: gotcha
---

# Inline SVG ignores theme unless shapes use CSS-variable classes, not hardcoded hex

When you make an HTML page theme-aware with CSS variables, **inline SVG diagrams do NOT follow the theme if their shapes carry hard-coded colors** — e.g. `fill="#1c2230"`, `stroke="#2b3444"`, `<text fill="#e6edf3">`. Those presentation attributes are literal and never read the `:root` variables, so the diagram stays in its baked-in colors (usually dark) while the page around it flips. Same trap for `<marker>` arrowheads (their `<path fill="...">` is fixed) and for any element styled with hard-coded values via inline `style`.

Fix: drive every SVG shape through **CSS classes that reference the variables**, applied in a stylesheet (not as presentation attributes):
```css
svg text{font-family:var(--sans)}
.s-box{fill:var(--panel2);stroke:var(--line)}
.s-ink{fill:var(--ink)}  .s-bad{fill:var(--bad)}
.ln-mut{stroke:var(--muted);fill:none}
.mk-bad{fill:var(--bad)}            /* marker <path> */
```
`fill`/`stroke` set via CSS **do** resolve `var()`; the same value written as an SVG attribute does not (attribute `fill="var(--x)"` is unreliable across browsers — use the CSS class instead). Define arrow `<marker>`s once in a shared hidden `<svg><defs>` so every diagram can `marker-end="url(#id)"` them and they recolor via their class.

Second, related gotcha in the same page: a **`@media (prefers-color-scheme)` rule cannot be overridden by a manual toggle** — a log/code box styled only in a media block stays put when the user clicks the theme button. Route those colors through the same `:root[data-theme]` variables instead.

Diagnostic tell: "the page flips but the diagrams / one box stay dark" ⇒ hunt for hard-coded hex in SVG attributes and in `@media`-only rules. Discovered building the luz-docs 800k perf report — see [[Theme toggle that overrides prefers-color-scheme via data-theme on root]].

## Related

- [[Theme toggle that overrides prefers-color-scheme via data-theme on root]]

%% ai-graph-start %%

**Related notes:**
- [[Theme toggle that overrides prefers-color-scheme via data-theme on root]]
- [[HTML report theme toggle reload gotcha]]
- [[Theme shared overlays with CSS-variable-backed Tailwind classes, not hardcoded colors]]
- [[Embed generated SVG in an artifact via img data-URI to isolate its styles]]
- [[Naive @import strip regex ate the root block (unstyled artifact)]]

%% ai-graph-end %%