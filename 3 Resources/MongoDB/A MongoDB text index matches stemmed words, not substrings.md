---
title: "A MongoDB text index matches stemmed words, not substrings"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: S1 — MongoDB $text Index (2026-06-25)"
tags: [mongodb, full-text-search, indexing, inverted-index, search, luz-docs]
---

# A MongoDB text index matches stemmed words, not substrings

A MongoDB **text index** is an **inverted index** — it maps *token → documents containing it*, so a word lookup is an `IXSCAN` instead of a collection scan. What makes it surprising is the **analysis pipeline** every indexed string goes through at build time:

1. **Tokenize** on word boundaries and punctuation
2. **Lowercase** (text search is always case-insensitive)
3. **Diacritic-fold** — `café → cafe` (text index v3)
4. **Stop-word removal** — language-specific noise words (`the`, `and`, `der`, `die`, …)
5. **Stem** — `invoices → invoic`, `running → run`

Query terms go through the *same* pipeline, so they meet the indexed terms in the middle.

**The consequence that decides whether you can use it: `$text` matches whole stemmed words, never substrings.** Replacing a regex `$or` (a "contains" search) with `$text` is the lowest-effort fully-native fix and comfortably hits sub-second at 128k documents — but it **silently changes the product behaviour**. A user searching `voic` stops finding `invoice`. This is a product decision disguised as an index change.

Two levers worth knowing:

- `default_language: "none"` turns **off** stemming and stop-words — useful for identifiers, filenames, and mixed-language corpora where stemming does more harm than good.
- `weights` per field make relevance ranking sane (`title: 10`, `documentTextContent: 1`).

When substring semantics are non-negotiable, the native alternative is a materialised n-gram/trigram field rather than `$text`.

## Related

- [[An index only helps an aggregation before the first group, unwind, or lookup]]
- [[Bounded bucketed hashing caps trigram index entries per document]]

## Related

- [[An index only helps an aggregation before the first group]]
- [[unwind]]
- [[or lookup]]
