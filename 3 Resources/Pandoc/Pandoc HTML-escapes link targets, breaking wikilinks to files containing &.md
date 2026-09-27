---
title: "Pandoc HTML-escapes link targets, breaking wikilinks to files containing &"
created: 2026-09-27
type: gotcha
status: seedling
source: "session 2026-09-27 Confluence vault import"
tags: [pandoc, obsidian, markdown, html-entities, import, gotcha]
---

# Pandoc HTML-escapes link targets, breaking wikilinks to files containing &

When you convert HTML to Markdown and then rewrite the link targets into Obsidian wikilinks, filenames containing `&`, `<`, or `>` silently break. Pandoc emits the link destination **HTML-escaped**, so a file genuinely named

```
071025 - Gravity & AT Exchange.pptx
```

arrives in the Markdown as `071025 - Gravity &amp; AT Exchange.pptx`. Build the embed straight from that string and you get `![[… &amp; …]]`, which matches nothing on disk. Obsidian renders it as an unresolved link rather than an error, so it looks fine until you click it.

The fix is to decode entities *after* URL-decoding and *before* constructing the link:

```js
const unent = s => String(s)
  .replace(/&amp;/g, '&').replace(/&lt;/g, '<').replace(/&gt;/g, '>')
  .replace(/&quot;/g, '"').replace(/&#39;/g, "'");

const embed = f => `![[${pageId}-${unent(decodeURIComponent(f.trim()))}]]`;
```

Order matters: `decodeURIComponent` first (handles `%20`), then entity decoding. Doing it the other way can turn a literal `&amp;` that was *meant* to be in the filename into `&` — rare, but the asymmetry is worth knowing.

> [!warning] Ampersand is the one that bites
> `&` is legal in filenames on every major filesystem and common in real document names ("Gravity & AT Exchange", "Terms & Conditions"), so this is not an edge case you can ignore. In my run it accounted for 12 of ~2,700 embeds — small enough to miss by eyeballing, large enough to matter.

**Detection:** never trust that generated links resolve. After a bulk import, walk every `![[target]]` and assert the target exists in the attachment pool:

```js
for (const m of text.matchAll(/!\[\[([^\]|#]+)/g))
  if (!attachments.has(m[1].trim())) broken.push(m[1].trim());
```

That check is what surfaced this; without it the import reports success and the breakage is only found months later by a human clicking a dead image.

Related: [[Pandoc gfm-raw_html silently replaces complex tables with [TABLE]|Pandoc gfm-raw_html silently replaces complex tables with [TABLE]]] — same family of failure, where a converter's output is plausible enough to pass an eyeball test but wrong.

## Related

- [[Pandoc gfm-raw_html silently replaces complex tables with [TABLE]|Pandoc gfm-raw_html silently replaces complex tables with [TABLE]]]
