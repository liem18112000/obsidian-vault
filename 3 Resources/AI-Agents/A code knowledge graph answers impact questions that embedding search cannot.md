---
title: "A code knowledge graph answers impact questions that embedding search cannot"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Code Knowledge Graph - Apply in AI Test Driven (2026-03-25)"
tags: [code-knowledge-graph, ai-agents, testing, static-analysis, tree-sitter, bdd]
---

# A code knowledge graph answers impact questions that embedding search cannot

A **code knowledge graph** turns source into a queryable structure: **nodes** are code entities (File, Class, Function, Type, Test, plus BDD `Step`, `Feature`, `Scenario`) and **edges** are relationships (`CALLS`, `IMPORTS`, `CONTAINS`, `INHERITS`, `TESTED_BY`, `IMPLEMENTS_STEP`).

This buys an agent three things that grep and embeddings cannot:

- **Blast-radius analysis** — bidirectional BFS from a changed entity, with configurable depth and a node cap, answers "what could this change break?" structurally rather than by guessing from names.
- **Coverage gaps as a query** — `find_untested_functions()` is just "functions with no inbound `TESTED_BY` edge". Coverage tools tell you which lines ran; the graph tells you which functions nothing even claims to test.
- **Test-to-code linkage across the Gherkin boundary** — `IMPLEMENTS_STEP` edges connect `.feature` scenarios to the Python `@given`/`@when`/`@then` definitions, so a scenario can be traced to the production code it actually exercises.

The construction is unremarkable on purpose: **tree-sitter** ASTs for Python/Java/JS/TS/Go, regex for Gherkin, stored behind one API over either SQLite (local, WAL) or Firestore (serverless).

Why this beats embedding search for *impact* questions: embeddings find code that is **similar**, the graph finds code that is **connected**. A caller three hops away is rarely similar to what you changed — and it is exactly what breaks.

## Related

- [[Incremental graph builds hash-skip unchanged files and re-parse only their dependents]]
- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]]

## Related

- [[Incremental graph builds hash-skip unchanged files and re-parse only their dependents]]
