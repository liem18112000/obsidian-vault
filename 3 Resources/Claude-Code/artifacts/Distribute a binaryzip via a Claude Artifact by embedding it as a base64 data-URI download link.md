---
ai_hash: a5dd1a1444935ac8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-18
entities: []
source: session 2026-08-18
status: seedling
tags:
- claude-code
- artifacts
- html
- base64
- data-uri
- technique
title: Distribute a binary/zip via a Claude Artifact by embedding it as a base64 data-URI
  download link
type: howto
---

# Distribute a binary/zip via a Claude Artifact by embedding it as a base64 data-URI download link

The Artifact tool publishes an HTML/Markdown **web page**, not a raw file host — there is no way to get a direct download URL for an arbitrary binary like a `.zip`. To hand a user a downloadable file anyway, embed the file **inside** the published page:

1. base64-encode the file (e.g. `base64 -w0 file.zip`).
2. Put it on an anchor as a data-URI with the `download` attribute:
   `<a download="file.zip" href="data:application/zip;base64,AAAA...">Download</a>`.

The whole thing is client-side, so the strict Artifact CSP (which blocks external hosts) does **not** interfere — a `data:` URI is not a network request. The base64 payload counts toward the 16 MB rendered-page limit (base64 inflates size ~33%), so this suits small-to-medium files, not huge ones.

Build tip: keep the HTML as a template with a `__B64__` placeholder and inject the base64 with a tiny script (assert the placeholder existed and is gone afterward) rather than pasting a 20k-char string by hand.

Related: [[Luz skills read shared env-selector ~.claudeskills_context (not bundled when porting a skill)]] — this technique was used to publish the luz-ship-ivy skill bundle zip.

## Related

- [[Luz skills read shared env-selector ~.claudeskills_context (not bundled when porting a skill)]]

%% ai-graph-start %%

**Related notes:**
- [[claude.ai Artifact iframe sandbox blocks data-URI downloads]]
- [[Embed generated SVG in an artifact via img data-URI to isolate its styles]]

%% ai-graph-end %%