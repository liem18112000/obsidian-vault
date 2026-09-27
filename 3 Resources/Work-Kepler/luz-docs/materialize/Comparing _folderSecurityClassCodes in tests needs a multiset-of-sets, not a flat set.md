---
ai_hash: 5206a83334caf3f5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-08
entities: []
source: session 2026-06-08 LUZ-154157
status: seedling
tags:
- luz-docs
- materialize
- testing
- python
- gotcha
title: Comparing _folderSecurityClassCodes in tests needs a multiset-of-sets, not
  a flat set
type: lesson
---

# Comparing _folderSecurityClassCodes in tests needs a multiset-of-sets, not a flat set

Because the Luz materialized field `_folderSecurityClassCodes` is a **list of lists** (one inner list per folder, duplicates kept, outer order not guaranteed), asserting it in tests with a flat `set()` is wrong — `set()` on nested lists raises (unhashable), and even after flattening it would collapse duplicate folders and lose the per-folder structure.

Compare it as a **multiset of sets**: normalize both sides by sorting each inner list (deduped) and sorting the outer list while **keeping duplicate inner lists**. This is order-insensitive on the outer list but still distinguishes `[["X"],["X"]]` from `[["X"]]`.

Helper used in `features/steps/delete_folder_materialized_steps.py`:

```python
def _norm_nested(value):
    return sorted(tuple(sorted(set(inner or []))) for inner in (value or []))
```

`tuple(...)` makes each inner hashable/comparable; sorting the outer gives order-insensitive equality; using a list (not a set) for the outer keeps duplicates.

## Related
[[Luz _folderSecurityClassCodes is a list-of-lists, one inner list per folder]]

## Related

- [[Luz _folderSecurityClassCodes is a list-of-lists, one inner list per folder]]

%% ai-graph-start %%

**Related notes:**
- [[Luz _folderSecurityClassCodes is a list-of-lists, one inner list per folder]]
- [[luz-docs folder delete verifies document security classes with one limit-1 Mongo query per folder]]
- [[Luz delete-folder tests can only delete public folders, not ones carrying a security class]]
- [[Interaction-style mocks hide ordering bugs that a stateful in-memory fake exposes]]
- [[Unit-testing FolderService recoverFolder requires per-collection stubs because process objects call back into the real service]]

%% ai-graph-end %%