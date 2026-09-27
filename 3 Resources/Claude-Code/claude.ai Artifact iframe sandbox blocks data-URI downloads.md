---
title: "claude.ai Artifact iframe sandbox blocks data-URI downloads"
created: 2026-09-21
type: lesson
status: seedling
source: "session 2026-09-21 run-7bcfe335"
tags: [artifact, claude-code, html, sandbox, gotcha]
---

# claude.ai Artifact iframe sandbox blocks data-URI downloads

GOTCHA: claude.ai Artifact pages render inside a sandboxed iframe that does NOT grant `allow-downloads`. So an `<a download href="data:...">` link (the test-agent report deliverables/fixtures used this) silently does nothing when clicked in the Artifact viewer — the browser blocks sandboxed data-URI/`download` navigations. It works fine when the same HTML file is opened directly. Fallback pattern (test-agent html._dl): pair every download link with an inline, collapsed <details><pre> copy of the same content, so the data is always retrievable (copyable) inside the sandbox. Do NOT claim a data-URI download "works in the Artifact viewer".
