---
title: "A note title containing a colon or slash breaks links that use the raw title"
created: 2026-09-27
type: lesson
status: seedling
source: "session 2026-09-27 (Confluence-Wiki link repair)"
tags: [obsidian, wikilinks, filenames, sanitization, gotcha, automation]
---

# A note title containing a colon or slash breaks links that use the raw title

Note-creation tools sanitize filenames — `:` `/` `<` `>` `"` are illegal on Windows or break `[[wikilinks]]`, so they get stripped. But a link **written from the original title** still carries them, and then it points at a file that does not exist:

| written in the link | file actually on disk |
|---|---|
| `[[Testing Tool Comparison: Playwright vs. k6]]` | `Testing Tool Comparison Playwright vs. k6.md` |
| `[[Ionic/angular]]` | `Ionic angular.md` |
| `[[await fetch() vs FetchBuilder<>()]]` | `await fetch() vs FetchBuilder ().md` |

19 links in one index note broke exactly this way. The generator wrote the link from the title and the file from the *sanitized* title, and nothing reconciled the two.

**Fix when generating links programmatically: derive the link from the filename you actually wrote, never from the title you started with.** Keep the pretty title as the alias — `[[sanitized-name|Original: Title]]` — so the prose still reads correctly.

A sibling failure with the same root: **a comma in a title split into two links.** `[[Confluence CQL search paginates by opaque cursor, not start offset]]` was authored as two separate bullets, `[[…opaque cursor]]` and `[[not start offset]]`, neither of which resolves. Any title containing list-like punctuation invites this when links are written by hand or by a model.

Detection is cheap and worth automating: normalise both sides aggressively (lowercase, drop every non-alphanumeric) and look for a unique match — that recovers the intended target for nearly all of these without guessing.

## Related

- [[Obsidian resolves short wikilinks by basename across the whole vault]]
- [[Verify a migration by reference parity with the source, not internal consistency]]

## Related

- [[Obsidian resolves short wikilinks by basename across the whole vault]]
