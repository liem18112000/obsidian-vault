---
ai_hash: edd28a5c15abf2ab
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
aliases:
- GFM heading anchors
- Markdown TOC anchor links
created: 2026-08-29
entities: []
source: session 2026-08-29 deployments README diagrams index
status: seedling
tags:
- markdown
- github
- gfm
- documentation
- gotcha
title: GitHub Markdown heading-anchor slug rules
type: reference
---

# GitHub Markdown heading-anchor slug rules

GitHub Flavored Markdown auto-generates a URL fragment (`#anchor`) for every heading. To link to a heading within the same document, derive the slug by this algorithm:

1. **Lowercase** the heading text.
2. **Strip** every character that is *not* a letter, number, space, or hyphen — so `` ` ``, `.`, `(`, `)`, `&`, `·`, and em-dash `—` are all removed, **but the spaces that surrounded them stay**.
3. **Replace spaces with hyphens**.

The gotcha: when punctuation sat between two spaces (e.g. ` — ` or ` & `), removing it leaves **two** adjacent spaces, which collapse into a **double hyphen** in the slug. Miss this and the anchor 404s silently (the link just scrolls nowhere).

Worked examples (from `deployments/README.md`):

| Heading | Anchor |
|---|---|
| `## One-shot deploy — ` + backtick`deploy-all.sh`backtick | `#one-shot-deploy--deploy-allsh` |
| `### 6 · Rollback & release history` | `#6--rollback--release-history` |
| `## Continuous Delivery (CD)` | `#continuous-delivery-cd` |

Note in the first example the `.` in `deploy-all.sh` vanishes (`allsh`), and both `—` and the backtick produce the `--`.

Verify anchors by grepping the target headings actually exist before shipping a table-of-contents / index that deep-links to them. Duplicate heading text gets a `-1`, `-2`, … suffix (github-slugger dedup).

%% ai-graph-start %%

**Related notes:**
- [[Resolving a wikilink by basename truncates titles containing a slash]]

%% ai-graph-end %%