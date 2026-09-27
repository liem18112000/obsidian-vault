---
title: "Injecting a custom Quartz component when the engine is cloned fresh in CI"
created: 2026-09-06
type: howto
status: seedling
source: "session 2026-09-06 docs-site chatbot"
tags: [quartz, static-site, ci, component, ssr, spa]
---

# Injecting a custom Quartz component when the engine is cloned fresh in CI

Quartz v4 sites that DON'T vendor the engine (CI clones it at a pinned tag and injects only quartz.config.ts + quartz.layout.ts) can still add a custom component without forking the engine:

1. Keep the component OUTSIDE the engine (e.g. docs-site/quartz-components/DocsChatbot.tsx + scripts/*.inline.ts + styles/*.scss).
2. In CI (and local preview), COPY it into the cloned engine's quartz/components/ (+ scripts/ + styles/) right after copying config/layout.
3. In quartz.layout.ts, import it by RELATIVE PATH — `import DocsChatbot from "./quartz/components/DocsChatbot"` — and use `afterBody: [DocsChatbot()]`. Importing directly avoids editing the engine's components/index.ts barrel (which the fresh clone would overwrite anyway).

Component wiring: `Chatbot.afterDOMLoaded = script` (from `import script from "./scripts/x.inline"` — Quartz bundles .inline.ts for the browser and hands you the source string) and `Chatbot.css = style` (from `import style from "./styles/x.scss"`).

Two gotchas:
- **Build-time config -> browser:** the .tsx renders server-side in Node, so it CAN read `process.env` (bake values via a CI env var with a default). The bundled inline script runs in the BROWSER and CANNOT read process.env — so pass config from the .tsx to the script via `data-*` attributes on the rendered element, and read them client-side.
- **SPA navigation:** with `enableSPA: true`, afterBody re-renders on every client nav. Build the widget ONCE and append it to `document.body` (guard with a window flag), so it isn't duplicated and survives navigation; leave only a small config marker in the afterBody slot.

Validate locally exactly like CI: clone the engine at the pinned tag, inject config+layout+component, run collect.mjs, `npx quartz build`, then grep the emitted public/ for your element id + baked data attrs. Relates to [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]].

## Related

- [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]]
