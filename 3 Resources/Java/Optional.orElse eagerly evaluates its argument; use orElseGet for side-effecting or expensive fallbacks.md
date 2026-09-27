---
ai_hash: dab3b9ad3caf0d63
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-07
entities: []
source: luz_docs_import folder-dup debug 2026-08-07
status: seedling
tags:
- java
- optional
- gotcha
- idempotency
- luz-docs-import
title: Optional.orElse eagerly evaluates its argument; use orElseGet for side-effecting
  or expensive fallbacks
type: gotcha
---

# Optional.orElse eagerly evaluates its argument; use orElseGet for side-effecting or expensive fallbacks

`Optional.orElse(x)` evaluates `x` **eagerly and unconditionally** — the argument is computed before orElse runs, whether or not the Optional is present. `orElseGet(supplier)` is **lazy** — the supplier runs only when the Optional is empty. They look interchangeable but differ whenever the fallback is expensive or has side effects.

Gotcha that bit luz_docs_import folder dedup: 
```java
Optional.ofNullable(getFolderByPath(...))                      // may find an existing folder
    .orElse(viewControllerService.createFolder(...));          // BUG: create call fires every time
```
Because orElse's argument is always evaluated, `createFolder` was invoked for every folder even when an existing one was found. On a first import it looked fine (nothing existed, the created folder was the one used); on **re-import** getFolderByPath returned the existing folder AND createFolder still ran as a discarded side effect — creating duplicate folders server-side on every re-run. Files deduped correctly (separate skip-partition path); folders duplicated solely because of this. Fix: `orElseGet(() -> viewControllerService.createFolder(...))`.

Rule of thumb: if the fallback is a method call that does I/O, mutates state, or is costly, use `orElseGet`; reserve `orElse` for already-computed constants/values. The same eager-vs-lazy trap applies to `Objects.requireNonNullElse` vs `requireNonNullElseGet`, and Map `getOrDefault` vs `computeIfAbsent`.

Related: [[luz_docs_import]], [[luz_jsonstore find projectsortcollation params must omit outer braces (server wraps them)|luz_jsonstore find: project/sort/collation params must omit outer braces (server wraps them)]].

## Related

- [[luz_docs_import]]

%% ai-graph-start %%

**Related notes:**
- [[Hand-rolled Optional.or fallback chain replaces CDI @Fallback]]
- [[empty-object-not-null sentinel defeats Optional.ofNullable null-guards]]
- [[putIfAbsent(Supplier) runs the loader under a global write lock]]
- [[luz_docs_import idempotent re-import replaces view-controller search-based file dedup]]
- [[luz-docs getDocumentById returns empty object not null for missing docs]]

%% ai-graph-end %%