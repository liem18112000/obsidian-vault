---
ai_hash: 60114d6c1af5e322
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities:
- test-agent-v2 run
- ePost design
- client-side Python script
- memory bank
- common.testplan.memory
- common.interrogate.pack.load_pack
- design-system tokens
- colors_and_type.css
- brand fonts
- SwissPostSans woff2
- logo svg
- HTML
- note.synopsis
- raw markup
- re.sub
- jira body
- diagrams
- common.testplan.diagrams.build_diagrams
- diagrams.json
- mermaid
- Artifact viewer
- CDN mermaid loader
- ePost brand rules
- '#FFCC00'
- yellow
- big hero
- black
- primary buttons
- flat design
- formal sentence-case copy
- no emoji
- Swiss date/number formatting
source: session 2026-09-21
status: seedling
tags:
- test-agent
- epost
- rendering
- howto
- mermaid
title: Render a test-agent run in ePost design client-side from the bank
type: howto
---

# Render a test-agent run in ePost design client-side from the bank

To render a test-agent-v2 run in a house design (e.g. ePost) the NEW way (agent=data, client renders): write a small client-side Python script (scratchpad, NOT repo) that (a) reads the runs persisted data straight from the memory bank (common.testplan.memory read_* + common.interrogate.pack.load_pack for sources), (b) inlines the design-system tokens (colors_and_type.css :root block, minus @font-face/@import) + embeds the brand fonts as base64 data: URIs (SwissPostSans woff2 are ~26KB each — cheap), inlines the logo svg, and (c) emits self-contained HTML. GOTCHAS: strip raw markup from note.synopsis with re.sub(r"<[^>]*>?"," ",s) (the >? also kills a dangling unclosed tag from jira body[:1500] cut mid-tag); REGENERATE diagrams locally via common.testplan.diagrams.build_diagrams(plan, coverage) instead of the persisted diagrams.json when the deployed image that implemented the run predates a diagram fix; mermaid renders in the Artifact viewer via <pre class="mermaid"> (add a cdn mermaid loader for standalone viewing). ePost brand rules that matter: yellow #FFCC00 big hero only, black primary buttons, flat (no shadows), formal sentence-case copy, no emoji, Swiss date/number formatting.

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 persists diagram-as-code + get_deliverables tool]]
- [[Naive @import strip regex ate the root block (unstyled artifact)]]
- [[test-agent-v2 always-enriched HTML report generator]]
- [[test-agent-v2 retired agent-side HTML renderers — agent is data-only, client renders]]

**Relations:**
- client-side Python script — *renders* — test-agent-v2 run
- test-agent-v2 run — *is rendered in* — ePost design
- client-side Python script — *reads data from* — memory bank
- client-side Python script — *uses* — common.testplan.memory
- client-side Python script — *uses* — common.interrogate.pack.load_pack
- client-side Python script — *inlines* — design-system tokens
- design-system tokens — *are from* — colors_and_type.css
- client-side Python script — *embeds* — brand fonts
- brand fonts — *include* — SwissPostSans woff2
- client-side Python script — *inlines* — logo svg
- client-side Python script — *emits* — HTML
- client-side Python script — *strips* — raw markup
- raw markup — *is from* — note.synopsis
- client-side Python script — *uses* — re.sub
- re.sub — *handles issues from* — jira body
- client-side Python script — *regenerates* — diagrams
- diagrams — *are regenerated via* — common.testplan.diagrams.build_diagrams
- diagrams — *are persisted in* — diagrams.json
- mermaid — *renders in* — Artifact viewer
- CDN mermaid loader — *enables standalone viewing of* — mermaid
- ePost design — *has* — ePost brand rules
- ePost brand rules — *specify color* — #FFCC00
- #FFCC00 — *is* — yellow
- yellow — *is for* — big hero
- ePost brand rules — *specify color* — black
- black — *is for* — primary buttons
- ePost brand rules — *include* — flat design
- ePost brand rules — *include* — formal sentence-case copy
- ePost brand rules — *include* — no emoji
- ePost brand rules — *include* — Swiss date/number formatting

%% ai-graph-end %%