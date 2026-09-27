---
title: "Dead-code refcount scans flag intentional seams as unused; vet before deleting"
created: 2026-09-08
type: lesson
status: seedling
source: "test-agent-v2 common/ cleanup, session 2026-09-08"
tags: [dead-code, refactoring, cleanup, testing, technique]
---

# Dead-code refcount scans flag intentional seams as unused; vet before deleting

A pure reference-count scan (grep every symbol, flag zero-external-refs / test-only) is the right *first pass* for dead-code cleanup, but it over-reports: several "unused" or "test-only" symbols are deliberate, not cruft. Before deleting, distinguish three kinds it mislabels:

1. **API-symmetry pairs** — e.g. `read_index`/`read_code_meta` are the read side of a store whose write side (`store_code_graph`) is live. No production code reads yet, so they scan as test-only, but they are the natural query counterpart and tests use them to verify writes. Deleting forces the test to poke at raw storage (implementation detail) — worse coverage for a few LOC.
2. **Live-triad members** — e.g. `from_decisions` alongside `from_gather`/`from_implement` (a signal-collector triad). Two are wired; the third (refine-capture) is not wired *yet*. It is unfinished-but-intentional, and it drives the canonical e2e integration test.
3. **Test-driver helpers** — symbols used *functionally* inside a broader test (to build fixtures/inputs), not merely asserted on. Removing them means rewriting the host test.

Genuine cruft (safe to delete): symbols with zero refs *including tests* (dead wrappers, orphaned enum constants, dead CLI entrypoints), and duplicate module-level aliases that every consumer re-derives locally.

Rule: after the refcount pass, read each flagged symbol's call sites and ask "is this an unwired/incomplete feature or a query/symmetry counterpart?" — if yes, keep (or ask), do not auto-delete. Also: a partially-wired feature whose *plugin/callback is a stub* (recall path) is unfinished, not obsolete.
