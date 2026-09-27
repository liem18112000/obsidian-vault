---
ai_hash: 27abd2be58d71a46
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: session 2026-09-27 (vault-wide link repair)
status: seedling
tags:
- obsidian
- wikilinks
- automation
- vault-maintenance
- refactoring
- safety
title: Repair broken links only when exactly one candidate matches, and iterate to
  a fixed point
type: howto
---

# Repair broken links only when exactly one candidate matches, and iterate to a fixed point

Repairing 1347 broken wikilinks by hand is impossible; rewriting them by fuzzy guess corrupts a vault. The rule that makes automation safe is narrow: **rewrite only when the target resolves to exactly ONE existing note. Otherwise report and leave it alone.**

Three classes turned out to cover ~2/3 of the breakage, all provable:

| class | cause | example |
|---|---|---|
| **retarget** | link written from the raw title, file saved sanitized | `[[Two-phase RAG UX: fast first]]` → the file without the `:` |
| **rejoin** | a comma-titled note authored as **two adjacent links** | `[[…Ivy 10.0.15]]` + `[[not 12]]` → one link |
| **repath** | stale folder in a full-path link, basename still unique | `[[3 Resources/AI/…/Claude Code hooks event model]]` → `[[Claude Code hooks event model]]` |

Matching is exact-after-normalisation (lowercase, strip every non-alphanumeric), **not** fuzzy or edit-distance. That is what produced **zero ambiguous cases** across 14 000 links — a collision would require two notes with identical alphanumerics, which effectively means they are the same note.

Two things that are easy to get wrong:

- **Don't touch unmatched links.** In a Zettelkasten an unresolved link is a *deliberate* placeholder for a note you intend to write. 455 survived here and should have. "Broken" and "not yet written" look identical to a checker and are opposites in intent.
- **Iterate to a fixed point.** Fixing one link can unblock another — a rejoin collapses two links into one whose target then matches a rename. The first pass made 645 fixes, a second found 2 more, a third found none. Run until zero.

Exclude `Templates/`, fenced code blocks, and code spans that *discuss* link syntax — `` `[[wikilink]]` `` in prose is not a broken link.

## Related

- [[A note title containing a colon or slash breaks links that use the raw title]]
- [[Obsidian resolves short wikilinks by basename across the whole vault]]
- [[Verify a migration by reference parity with the source, not internal consistency]]

## Related

- [[A note title containing a colon or slash breaks links that use the raw title]]
- [[Obsidian resolves short wikilinks by basename across the whole vault]]

%% ai-graph-start %%

**Related notes:**
- [[Comma-split wikilinks leave dead fragment links in Related blocks]]
- [[create_note.py --link comma-splits titles and silently breaks wikilinks]]
- [[Resolving a wikilink by basename truncates titles containing a slash]]
- [[A note title containing a colon or slash breaks links that use the raw title]]
- [[Measure a broken-link baseline before a mass vault refactor]]

%% ai-graph-end %%