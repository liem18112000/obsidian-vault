---
ai_hash: 5979ac104c324d2d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities: []
source: session 2026-09-21 run-7bcfe335
status: seedling
tags:
- artifact
- claude-code
- html
- sandbox
- gotcha
title: claude.ai Artifact iframe sandbox blocks data-URI downloads
type: lesson
---

# claude.ai Artifact iframe sandbox blocks data-URI downloads

GOTCHA: claude.ai Artifact pages render inside a sandboxed iframe that does NOT grant `allow-downloads`. So an `<a download href="data:...">` link (the test-agent report deliverables/fixtures used this) silently does nothing when clicked in the Artifact viewer — the browser blocks sandboxed data-URI/`download` navigations. It works fine when the same HTML file is opened directly. Fallback pattern (test-agent html._dl): pair every download link with an inline, collapsed <details><pre> copy of the same content, so the data is always retrievable (copyable) inside the sandbox. Do NOT claim a data-URI download "works in the Artifact viewer".

%% ai-graph-start %%

**Related notes:**
- [[Distribute a binaryzip via a Claude Artifact by embedding it as a base64 data-URI download link]]
- [[test-agent-v2 retired agent-side HTML renderers — agent is data-only, client renders]]
- [[claude.ai share links can be org-restricted and require login]]
- [[Artifacts render mermaid natively — never add a mermaid CDN script (CSP blocks it)]]
- [[Embed generated SVG in an artifact via img data-URI to isolate its styles]]

%% ai-graph-end %%