---
ai_hash: c83407280ce0f11b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: test-agent-v2 KGA agent-based restructure, session 2026-09-09
status: seedling
tags:
- python
- packaging
- refactoring
- imports
- circular-import
title: Turning a module into a package without breaking from-pkg-import-X (avoid the
  __init__ cycle)
type: howto
---

# Turning a module into a package without breaking from-pkg-import-X (avoid the __init__ cycle)

When you refactor a module `pkg/thing.py` into a package `pkg/thing/` (e.g. splitting `gather.py` into `gather/agent.py` + `gather/domain.py`), external callers that did `from pkg.thing import X` keep working **only if** `pkg/thing/__init__.py` re-exports `X`. Do that — the package `__init__` becomes the module's stable public surface, so you don't have to rewrite every external `from pkg.thing import X` importer.

**The cycle trap:** if `__init__.py` re-exports from a submodule (`from pkg.thing.agent import build`), and that submodule needs a sibling's symbols, the submodule must import the sibling **by submodule path** (`from pkg.thing.domain import helper`), NOT via the package (`from pkg.thing import helper`). Importing via the package re-triggers `pkg/thing/__init__.py` **while it is still being initialized** → partially-initialized-module ImportError / circular import. Rule: **package `__init__` may depend on submodules; submodules must depend only on other submodules, never on their own package `__init__`.**

Mechanics that made a ~25-file agent-based restructure safe:
- `git mv` every file/dir (preserves history; git shows them as renames R).
- Rewrite import paths with ordered `sed` over `src/ tests/` for the moved prefixes; leave the `from pkg.thing import` form alone and handle it with the `__init__` re-export instead.
- After moving, `ruff check --fix` re-sorts imports (moved paths change alphabetical order).
- The guard: full test suite must show the **same pass/skip counts** as before (behavior-identical). Here 388 passed / 14 skipped, unchanged.

See [[Test an ADK LlmAgent(output_schema=) offline with a BaseLlm fake yielding canned JSON]].

**Related sed gotcha (moving a module into a subpackage):** a dotted-path rewrite `s#pkg\.X#pkg.sub.X#` catches `import pkg.X` and `from pkg.X import y`, but MISSES the name-form `from pkg import X` (there the token is `pkg import X`, not `pkg.X`). Grep BOTH forms after the move: `from <pkg> import <moved_name>\b` as well as `<pkg>\.<moved_name>`. Collection-time ImportError ("cannot import name X from pkg") is the tell.

%% ai-graph-start %%

**Related notes:**
- [[Convert a Python module to a package without breaking importers via re-exporting __init__]]
- [[Break a package import cycle by moving annotation-only imports under TYPE_CHECKING]]
- [[Monkeypatched module attributes are a hidden breakage risk when a module becomes a package]]
- [[Flat-import Python modules can be relocated together without rewriting imports]]
- [[Extracting a shared utils package - classify by whether code knows source semantics]]

%% ai-graph-end %%