---
ai_hash: 0f2cdec6a9ff5a0b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: session 2026-09-27 (Confluence -> Obsidian import)
status: seedling
tags:
- obsidian
- wikilinks
- embeds
- migration
- gotcha
- naming
title: Obsidian resolves short wikilinks by basename across the whole vault
type: lesson
---

# Obsidian resolves short wikilinks by basename across the whole vault

An Obsidian embed written short — `![[image-20251126-070727.png]]` — is **not** resolved relative to the linking note. Obsidian searches the **entire vault** for a file with that basename. If exactly one exists, you get it. If several do, Obsidian silently picks one (nearest path wins), and the note renders someone else's image with no error.

This makes short links a **vault-global namespace claim**, which matters when importing a batch of files. Checking that basenames are unique *within the import* is not enough — a collision with any pre-existing file anywhere in the vault redirects the link.

The safe rule when generating links programmatically:

```js
// index the WHOLE vault, not just the incoming set
const hits = byName.get(basename) ?? [];
return hits.length === 1 ? `![[${basename}]]` : `![[${fullVaultRelativePath}]]`;
```

Full-path embeds (`![[3 Resources/.../attachments/page/img.png]]`) are verbose but unambiguous, so they are the right default whenever uniqueness is not proven.

Why this is worth guarding rather than testing for later: the failure is **silent and plausible**. A link-checker reports it as resolved, because it *does* resolve — just to the wrong file. Only comparing each link against the file it was *supposed* to reach catches it. For an import of screenshots, a wrong-but-valid image can sit unnoticed indefinitely.

Timestamped Confluence exports (`image-YYYYMMDD-HHMMSS.png`) happen to be collision-free, but generic names — `screenshot.png`, `diagram.png`, `image.png` — are exactly the ones that collide.

## Related

- [[Verify a migration by reference parity with the source, not internal consistency]]

## Related

- [[Verify a migration by reference parity with the source, not internal consistency]]

%% ai-graph-start %%

**Related notes:**
- [[Verify a migration by reference parity with the source, not internal consistency]]
- [[Resolving a wikilink by basename truncates titles containing a slash]]
- [[A note title containing a colon or slash breaks links that use the raw title]]
- [[Repair broken links only when exactly one candidate matches, and iterate to a fixed point]]
- [[Measure a broken-link baseline before a mass vault refactor]]

%% ai-graph-end %%