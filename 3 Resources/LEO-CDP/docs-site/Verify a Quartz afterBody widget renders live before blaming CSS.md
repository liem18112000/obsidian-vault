---
ai_hash: e77d32acbc4674d4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-07
entities: []
source: session 2026-09-07 chatbot-gone investigation
status: seedling
tags:
- leo-cdp
- docs-site
- quartz
- playwright
- debugging
- css
title: Verify a Quartz afterBody widget renders live before blaming CSS
type: howto
---

# Verify a Quartz afterBody widget renders live before blaming CSS

The docs-site "Ask the Docs" RAG chatbot is a Quartz `afterBody` component (`quartz.layout.ts` → `afterBody: [DocsChatbot()]`). Its inline `afterDOMLoaded` script builds a floating launcher (`#docs-chat-launcher`) + panel and appends them to `<body>` **once**, guarded by `window.__docsChatbotReady` so it survives SPA navigation. Config (API + site base) is baked at build time into `#docs-chatbot-root` `data-api` / `data-site` attributes from `DOCS_AI_PUBLIC_URL` / `DOCS_SITE_BASE` in CI.

**Before blaming a CSS regression for a "the widget is gone" report, prove whether it is real or an illusion — with Playwright on the live page:**
1. Element exists in DOM (`getElementById`).
2. `getComputedStyle` → `display` / `visibility` / `opacity` are renderable.
3. `getBoundingClientRect` → box is inside the viewport (not pushed off-screen).
4. `document.elementFromPoint(centerX, centerY)` returns the element (or its own child), i.e. nothing overlays it.

If all four pass, the code is fine and the cause is **client-side**: a stale GitHub Pages cache of `postscript.js` (fix: hard refresh), or the launcher simply being low-contrast — the docs site styles it with `--secondary` (`#284b63`), not the frontend-admin apps bright indigo, so it is easy to overlook at bottom-right. In the 2026-09-07 investigation, all four checks passed and a screenshot showed the launcher present — it was never actually removed; recent commits were cosmetic only.

Related: [[Docs Site deploy-docs.yml detect job skips build unless docs paths change]]

## Related

- [[Docs Site deploy-docs.yml detect job skips build unless docs paths change]]

%% ai-graph-start %%

**Related notes:**
- [[Injecting a custom Quartz component when the engine is cloned fresh in CI]]
- [[Docs Site deploy-docs.yml detect job skips build unless docs paths change]]

%% ai-graph-end %%