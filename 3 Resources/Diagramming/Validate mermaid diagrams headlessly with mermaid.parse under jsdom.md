---
title: "Validate mermaid diagrams headlessly with mermaid.parse under jsdom"
created: 2026-09-23
type: howto
status: seedling
source: "session 2026-09-23"
tags: [mermaid, jsdom, nodejs, diagramming, validation, gotcha]
---

# Validate mermaid diagrams headlessly with mermaid.parse under jsdom

To check that ```mermaid diagrams parse (without a browser or the heavy mermaid-cli/chromium), call `mermaid.parse()` in Node under a jsdom DOM. `parse` throws on syntax errors and returns `{diagramType}` on success — enough to gate a doc before commit.

```js
import { JSDOM } from "jsdom";
const dom = new JSDOM("<!doctype html><html><body></body></html>");
globalThis.window = dom.window;
globalThis.document = dom.window.document;
// NOTE: do NOT set globalThis.navigator on Node 22 — it is a read-only getter and assigning throws.
const { default: mermaid } = await import("mermaid");
mermaid.initialize({ startOnLoad: false });
for (const block of blocks) {
  try { const r = await mermaid.parse(block, { suppressErrors: false }); console.log("OK", r.diagramType); }
  catch (e) { console.log("FAIL", e.message.split("\n")[0]); }
}
```

Deps: `npm i mermaid jsdom` (~tens of MB — far lighter than mermaid-cli, which needs Playwright chromium). Extract blocks with a regex: `/```mermaid\n([\s\S]*?)```/g`.

Gotchas:
- Node 22 exposes a read-only `navigator` global; assigning `globalThis.navigator = ...` throws `Cannot set property navigator` — just omit it, parse does not need it.
- This validates SYNTAX only, not rendered layout. Flowchart `\n` line-breaks in quoted node labels parse fine under mermaid v11 (flowchart-v2), which is what GitHub/GitBook run, so they are portable.
- A separate memory warns that mermaid **erDiagram** style/classDef can report valid but break on GitHub — a passing parse is necessary, not sufficient, for those.

Related: [[Embed Excalidraw in repo markdown render to SVG; the renderer does not auto-wrap bound text]].

## Related

- [[Embed Excalidraw in repo markdown render to SVG; the renderer does not auto-wrap bound text]]
