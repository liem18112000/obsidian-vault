---
ai_hash: 7ae0611ced22faab
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 deployments/proxy, 2026-08
status: seedling
tags:
- bash
- shell
- gotcha
- quoting
title: Apostrophe inside bash ${var:?message} breaks the parser
type: lesson
---

# Apostrophe inside bash ${var:?message} breaks the parser

An unescaped ASCII apostrophe inside a `${var:?message}` default/error word can throw the bash parser off — even when the whole expansion sits inside double quotes, or when the apostrophe is in a comment nearby. Symptom: a misleading `syntax error near unexpected token ...` pointing at a line far below the real cause (bash gets confused about where the next single-quoted region starts).

**Why:** bash tokenizes quote state across the `${...:?...}` word; a lone `'` reads as opening a single-quoted string, silently swallowing everything until the next `'`.

**How to apply:** in `${VAR:?...}` messages (and script comments in the same region) avoid a raw `'`. Rewrite ("Lets" not "Let's"), or use a typographic apostrophe (U+2019 `’`), which is just a literal byte. Hit this writing `: "${EMAIL:?set acme_email (Let's Encrypt ...)}"`.

Related: [[Ship shell env over SSH as one base64 blob to dodge arg-flattening]]

## Related

- [[Bash]]

%% ai-graph-start %%

**Related notes:**
- [[PowerShell here-string @'...'@ silently corrupts git commit messages in the Bash tool]]
- [[set -u trips on a bash array only populated on the failure path, making success exit non-zero]]
- [[ssh 'bash -s' flattens args, so empty middle args shift positionals]]
- [[Bash unquoted variable expansion re-splits on whitespace, breaking quoted args]]
- [[SSH flattens remote command args, so empty-string arguments collapse and shift positionals]]

%% ai-graph-end %%