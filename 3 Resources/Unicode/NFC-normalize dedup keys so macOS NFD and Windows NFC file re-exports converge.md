---
ai_hash: 34a5297836c9b1c2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities: []
source: luz_docs_import LUZ-158230 · 2026-08-11
status: seedling
tags:
- unicode
- normalization
- dedup
- i18n
title: NFC-normalize dedup keys so macOS NFD and Windows NFC file re-exports converge
type: lesson
---

# NFC-normalize dedup keys so macOS NFD and Windows NFC file re-exports converge

When a dedup/matching **key** is derived from a file path, normalize it to **Unicode NFC + forward-slashes + no leading slash** before storing/comparing. Otherwise the *same* file re-exported from macOS (which stores names decomposed, **NFD**) and Windows (composed, **NFC**) produces byte-different keys — e.g. `hợp-đồng.pdf` fails to match itself across producers — silently defeating dedup. Especially relevant for Vietnamese/diacritic filenames.

Normalize only the **key**; keep the raw on-disk name for pairing and display (within one export, a document and its sidecar mangle consistently, so on-disk pairing already survives — only the cross-producer key drifts).

```java
Normalizer.normalize(relativePath, Normalizer.Form.NFC).replace("\\","/"); // then strip a leading "/"
```

Docker does **not** fix this: the container normalizes the runtime, not the input ZIPs bytes.

Related: [[Content-addressed dedup with a unique index and insert-first is concurrency-correct]]

## Related

- [[Content-addressed dedup with a unique index and insert-first is concurrency-correct]]

%% ai-graph-start %%

**Related notes:**
- [[Content-addressed dedup with a unique index and insert-first is concurrency-correct]]
- [[Building a ZIP fixture to test NFCNFD + UTF-8-flag entry-name handling]]
- [[luz-docs-import dedup identity is the uploaded zip filename (importZipName)]]

%% ai-graph-end %%