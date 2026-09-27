---
ai_hash: 3e660afdc2dcf3bf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: session 2026-09-27 (Confluence -> Obsidian import)
status: seedling
tags:
- javascript
- regex
- gotcha
- encoding
- line-endings
title: JS regex dot excludes carriage return, so (.*)$ silently fails on CRLF lines
type: lesson
---

# JS regex dot excludes carriage return, so (.*)$ silently fails on CRLF lines

In JavaScript regex, `.` matches any character **except line terminators** — and `\r` is one of them, alongside `\n`, ` `, ` `. So a pattern ending in `(.*)$` cannot match a line that still carries a trailing `\r`: `.*` stops short of the `\r`, then `$` refuses to match because a character remains. The regex returns `null` with no error.

This bites hardest when you `split('\n')` a CRLF file and run line-wise transforms. Every line except the last of the file ends in `\r`, so **anchored** patterns silently no-op while **unanchored** ones keep working — producing a half-transformed result that looks like a logic bug, not an encoding bug.

```js
/^(#{1,5})\s+(.*)$/.exec("## Heading\r")   // → null   (not what you expect)
/^(#{1,5})\s+(.*)$/.exec("## Heading")     // → match
```

The tell: some transforms in the same loop fire and others don't. In my case a heading-dedupe branch (which compared `h[2].trim()`) worked on the one `\n`-terminated line while heading-demotion failed on all the `\r`-terminated ones — same loop, same regex, different outcomes purely by line ending.

**Fix:** normalize before you split, not per-line.

```js
const body = raw.replace(/\r\n?/g, '\n');   // first thing, before any parsing
```

`\r\n?` also catches lone classic-Mac `\r`. Doing it once up front is cheaper and safer than sprinkling `.trim()` or `\r?$` into every pattern — you will forget one.

## Related

- [[Windows Git Bash mangles non-ASCII to cp1252 breaking UTF-8]]

## Related

- [[Windows Git Bash mangles non-ASCII to cp1252 breaking UTF-8]]

%% ai-graph-start %%

**Related notes:**
- [[Line-removal regex must use rn when reading files with newline='']]
- [[Claude Code Bash tool collapses backslashes even inside quoted heredocs]]
- [[Windows PowerShell 5.1 reads BOM-less scripts as ANSI, breaking on em-dashes]]
- [[Windows Git Bash mangles non-ASCII to cp1252 breaking UTF-8]]
- [[Generate a git patch under autocrlf so git apply matches a CRLF worktree]]

%% ai-graph-end %%