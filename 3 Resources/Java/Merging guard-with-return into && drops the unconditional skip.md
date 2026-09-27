---
ai_hash: 8670fbecf151b003
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-06
entities: []
source: luz_docs_import incident 2026-08-06
status: seedling
tags:
- java
- control-flow
- refactoring-hazard
- linter
- gotcha
title: Merging guard-with-return into && drops the unconditional skip
type: lesson
---

# Merging guard-with-return into && drops the unconditional skip

A "simplification" that folds a nested guard into a single `&&` can silently change behavior when the inner block is followed by an **unconditional** statement (a `return`/`continue`/skip).

```java
// CORRECT: skip EVERY metadata file; only orphans are also recorded as rejected
if (isMetadata(file)) {
    if (isOrphan(file)) { reject(file); }
    return;                       // fires for metadata whether orphan or not
}
createDocument(file);

// WRONG (what an auto-simplifier produced): only orphan metadata skipped
if (isMetadata(file) && isOrphan(file)) {
    reject(file);
    return;                       // never reached for non-orphan metadata
}
createDocument(file);             // <-- non-orphan metadata now wrongly imported
```

The two are NOT equivalent: the `return` in the first form is unconditional inside `if(A)`, so it also covers the `A && !B` case. Folding to `if (A && B)` moves that case to the fall-through path.

Real incident (luz_docs_import, 2026-08): companion `*.metadata.json` files (which must be consumed as metadata, never imported) were being uploaded as binary documents on the recipient's eArchive. Root cause: an aggressive auto-formatter/linter rewrote `if(isMetadata){ if(isOrphan){reject;} return; }` into `if(isMetadata && isOrphan){reject; return;}`. Only orphan metadata was skipped; every metadata file that HAD a matching document fell through to createDocument.

Lesson: when a guard clause ends in an unconditional skip/return, do NOT let it be merged with the inner condition. Review linter/auto-simplify diffs on control flow, and prefer a test that asserts the A-and-not-B path is skipped.

## Related

- [[luz_docs_import]]

%% ai-graph-start %%

**Related notes:**
- [[A refactor that removes a method must grep tests for its name before merging]]
- [[Gate behavior changes must update tests asserting old fallthrough in the same commit]]
- [[empty-object-not-null sentinel defeats Optional.ofNullable null-guards]]
- [[luz-docs getDocumentById returns empty object not null for missing docs]]
- [[Encode a benign-error decision in a dedicated exception type, not a swallowed catch on a magic status code]]

%% ai-graph-end %%