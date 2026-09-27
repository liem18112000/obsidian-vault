---
title: "Artifacts render mermaid natively — never add a mermaid CDN script (CSP blocks it)"
created: 2026-09-23
type: lesson
status: seedling
source: "session 2026-09-23"
tags: [artifacts, mermaid, csp, gotcha]
---

# Artifacts render mermaid natively — never add a mermaid CDN script (CSP blocks it)

GOTCHA: Claude Artifacts render mermaid diagrams NATIVELY — put the diagram in a `<pre class="mermaid">...</pre>` block (HTML) or a ```mermaid fence (Markdown) and the platform draws it. Do NOT add a `<script type="module">import mermaid from "https://cdn.jsdelivr.net/...">` initializer: the Artifact CSP blocks all external hosts, so the CDN import silently fails (and is redundant). Same rule for any library/font/image in an artifact — inline it or use a native feature; nothing external loads. Rendered mermaid-as-code (.mmd flowcharts) from the testing-agent get_deliverables embeds cleanly this way. See [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]].
