---
title: "Verify a migration by reference parity with the source, not internal consistency"
created: 2026-09-27
type: lesson
status: seedling
source: "session 2026-09-27 (Confluence -> Obsidian import)"
tags: [migration, verification, data-integrity, testing, practices]
---

# Verify a migration by reference parity with the source, not internal consistency

After migrating content, "every link resolves" is a **weak** check. It proves the output is internally consistent — which a migration that dropped half its references would also pass, because whatever survived still points somewhere valid.

The strong check is **parity against the source**: for each item, compare the set of references in the original with the set in the output.

```
per page:  refs_in_source  vs  refs_in_output
  lost   = source \ output     ← dropped by the converter
  gained = output \ source     ← invented by the converter
```

Both directions matter. *Lost* catches a regex that missed a markup variant. *Gained* catches a rewrite rule firing too broadly and linking things that were never linked.

The distinction showed up concretely in this import: 833 attachment files on disk but only 660 embeds — a gap that looks like data loss. Parity showed **167/167 pages with identical references, 0 lost, 0 gained**, so the 166 unreferenced files were uploaded-but-never-embedded in Confluence itself. Without the source comparison there was no way to tell "the converter dropped these" from "they were never referenced".

Generalisation: **a migration has two failure modes, and only one is visible from the output.** Broken output is obvious; *silently thinner* output is not. Always keep the source readable long enough to diff against, and check counts per item rather than in aggregate — totals can match while individual items are scrambled.

## Related

- [[Obsidian resolves short wikilinks by basename across the whole vault]]

## Related

- [[Obsidian resolves short wikilinks by basename across the whole vault]]
