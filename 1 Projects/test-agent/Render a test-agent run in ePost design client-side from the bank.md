---
title: "Render a test-agent run in ePost design client-side from the bank"
created: 2026-09-21
type: howto
status: seedling
source: "session 2026-09-21"
tags: [test-agent, epost, rendering, howto, mermaid]
---

# Render a test-agent run in ePost design client-side from the bank

To render a test-agent-v2 run in a house design (e.g. ePost) the NEW way (agent=data, client renders): write a small client-side Python script (scratchpad, NOT repo) that (a) reads the runs persisted data straight from the memory bank (common.testplan.memory read_* + common.interrogate.pack.load_pack for sources), (b) inlines the design-system tokens (colors_and_type.css :root block, minus @font-face/@import) + embeds the brand fonts as base64 data: URIs (SwissPostSans woff2 are ~26KB each — cheap), inlines the logo svg, and (c) emits self-contained HTML. GOTCHAS: strip raw markup from note.synopsis with re.sub(r"<[^>]*>?"," ",s) (the >? also kills a dangling unclosed tag from jira body[:1500] cut mid-tag); REGENERATE diagrams locally via common.testplan.diagrams.build_diagrams(plan, coverage) instead of the persisted diagrams.json when the deployed image that implemented the run predates a diagram fix; mermaid renders in the Artifact viewer via <pre class="mermaid"> (add a cdn mermaid loader for standalone viewing). ePost brand rules that matter: yellow #FFCC00 big hero only, black primary buttons, flat (no shadows), formal sentence-case copy, no emoji, Swiss date/number formatting.
