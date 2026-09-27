---
ai_hash: 23575f17e5b53125
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-06
entities: []
source: session 2026-09-06 docs-site chatbot
status: seedling
tags:
- css
- cascade
- inheritance
- specificity
- theming
- gotcha
title: A directly-set CSS color beats an inherited one regardless of specificity
type: lesson
---

# A directly-set CSS color beats an inherited one regardless of specificity

In the CSS cascade, an **inherited** value is the weakest possible source — it loses to ANY rule that sets the property directly on the element, even a low-specificity element selector. So relying on inheritance to color text fails when a global rule targets that element type.

Concrete bug: a widget header set `color: var(--light)` on the container and let child `<p>` title/subtitle inherit it. The host site (Quartz) has a global `p { color: var(--darkgray) }`. That direct rule on the `<p>` overrode the inherited `--light`, so the title rendered dark-gray on a dark accent header → nearly invisible. Specificity wasn't even the issue — inheritance simply doesn't compete with a direct declaration.

Fix: set the color **directly on the element you control** (a class selector, e.g. `.docs-chat-title { color: var(--light) }`, specificity 0,1,0) — it beats the host's element rule (`p`, 0,0,1). 

General lesson when embedding a widget into a host page/theme you don't fully control: don't inherit text color for anything sitting on a custom background — set it explicitly on each text element, because the host's global `p`/`a`/`li`/`h*` rules will win over inheritance. Relates to [[Injecting a custom Quartz component when the engine is cloned fresh in CI]].

## Related

- [[Injecting a custom Quartz component when the engine is cloned fresh in CI]]

%% ai-graph-start %%

**Related notes:**
- [[Theme shared overlays with CSS-variable-backed Tailwind classes, not hardcoded colors]]
- [[Inline SVG ignores theme unless shapes use CSS-variable classes, not hardcoded hex]]

%% ai-graph-end %%