---
ai_hash: 7f5215e579d226e2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
source: session 2026-09-23
status: seedling
tags:
- artifacts
- mermaid
- csp
- gotcha
title: Artifacts render mermaid natively — never add a mermaid CDN script (CSP blocks
  it)
type: lesson
---

# Artifacts render mermaid natively — never add a mermaid CDN script (CSP blocks it)

GOTCHA: Claude Artifacts render mermaid diagrams NATIVELY — put the diagram in a `<pre class="mermaid">...</pre>` block (HTML) or a ```mermaid fence (Markdown) and the platform draws it. Do NOT add a `<script type="module">import mermaid from "https://cdn.jsdelivr.net/...">` initializer: the Artifact CSP blocks all external hosts, so the CDN import silently fails (and is redundant). Same rule for any library/font/image in an artifact — inline it or use a native feature; nothing external loads. Rendered mermaid-as-code (.mmd flowcharts) from the testing-agent get_deliverables embeds cleanly this way. See [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]].

%% ai-graph-start %%

**Related notes:**
- [[Embed generated SVG in an artifact via img data-URI to isolate its styles]]
- [[Validate mermaid diagrams headlessly with mermaid.parse under jsdom]]
- [[claude.ai Artifact iframe sandbox blocks data-URI downloads]]
- [[test-agent-v2 persists diagram-as-code + get_deliverables tool]]
- [[Mermaid render() leaks its error-bomb SVG into the DOM past a caught throw; fix with suppressErrorRendering]]

%% ai-graph-end %%