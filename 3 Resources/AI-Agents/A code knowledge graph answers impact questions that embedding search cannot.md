---
ai_hash: e2b7827175726078
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Code knowledge graph
- Source
- Nodes
- Code entities
- File
- Class
- Function
- Type
- Test
- BDD Step
- Feature
- Scenario
- Edges
- Relationships
- CALLS
- IMPORTS
- CONTAINS
- INHERITS
- TESTED_BY
- IMPLEMENTS_STEP
- Agent
- Grep
- Embedding search
- Blast-radius analysis
- BFS
- Changed entity
- Coverage gaps
- Query
- find_untested_functions()
- Coverage tools
- Lines
- Test-to-code linkage
- Gherkin boundary
- .feature scenarios
- Python definitions
- '@given'
- '@when'
- '@then'
- Production code
- Tree-sitter
- ASTs
- Python
- Java
- JS
- TS
- Go
- Regex
- Gherkin
- API
- SQLite
- Firestore
- Impact questions
- Similar code
- Connected code
- Caller
- Incremental graph builds hash-skip unchanged files and re-parse only their dependents
- Multi-agent systems trade a single capable agent for specialisation, isolation and
  parallelism
source: 'Confluence: Code Knowledge Graph - Apply in AI Test Driven (2026-03-25)'
status: seedling
tags:
- code-knowledge-graph
- ai-agents
- testing
- static-analysis
- tree-sitter
- bdd
title: A code knowledge graph answers impact questions that embedding search cannot
type: concept
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

%% ai-graph-start %%

**Related notes:**
- [[Code Knowledge Graph - Apply in AI Test Driven]]
- [[Incremental graph builds hash-skip unchanged files and re-parse only their dependents]]
- [[Graphify for code reviews without source access]]
- [[graphify offline workflow extract --code-only then cluster-only]]
- [[Multi-Agentic Architecture - Apply in AI Driven Testing]]

**Relations:**
- Code knowledge graph — *transforms* — Source
- Code knowledge graph — *composed of* — Nodes
- Code knowledge graph — *composed of* — Edges
- Nodes — *represent* — Code entities
- Code entities — *includes* — File
- Code entities — *includes* — Class
- Code entities — *includes* — Function
- Code entities — *includes* — Type
- Code entities — *includes* — Test
- Code entities — *includes* — BDD Step
- Code entities — *includes* — Feature
- Code entities — *includes* — Scenario
- Edges — *represent* — Relationships
- Relationships — *includes* — CALLS
- Relationships — *includes* — IMPORTS
- Relationships — *includes* — CONTAINS
- Relationships — *includes* — INHERITS
- Relationships — *includes* — TESTED_BY
- Relationships — *includes* — IMPLEMENTS_STEP
- Code knowledge graph — *provides benefits to* — Agent
- Code knowledge graph — *surpasses* — Grep
- Code knowledge graph — *surpasses* — Embedding search
- Code knowledge graph — *enables* — Blast-radius analysis
- Blast-radius analysis — *uses* — BFS
- Blast-radius analysis — *starts from* — Changed entity
- Code knowledge graph — *identifies* — Coverage gaps
- Coverage gaps — *identified by* — Query
- Query — *example* — find_untested_functions()
- find_untested_functions() — *finds* — Function
- Function — *lacks* — TESTED_BY
- Coverage tools — *report on* — Lines
- Code knowledge graph — *reports on* — Function
- Code knowledge graph — *enables* — Test-to-code linkage
- Test-to-code linkage — *crosses* — Gherkin boundary
- IMPLEMENTS_STEP — *connects* — .feature scenarios
- IMPLEMENTS_STEP — *connects to* — Python definitions
- Python definitions — *includes* — @given
- Python definitions — *includes* — @when
- Python definitions — *includes* — @then
- .feature scenarios — *exercises* — Production code
- Code knowledge graph — *constructed using* — Tree-sitter
- Tree-sitter — *generates* — ASTs
- ASTs — *for language* — Python
- ASTs — *for language* — Java
- ASTs — *for language* — JS
- ASTs — *for language* — TS
- ASTs — *for language* — Go
- Code knowledge graph — *constructed using* — Regex
- Regex — *for* — Gherkin
- Code knowledge graph — *stored via* — API
- API — *uses backend* — SQLite
- API — *uses backend* — Firestore
- SQLite — *is type* — local
- Firestore — *is type* — serverless
- Code knowledge graph — *answers* — Impact questions
- Embedding search — *finds* — Similar code
- Code knowledge graph — *finds* — Connected code
- Connected code — *example* — Caller
- Caller — *is not* — Similar code
- Code knowledge graph — *related to* — Incremental graph builds hash-skip unchanged files and re-parse only their dependents
- Code knowledge graph — *related to* — Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism

%% ai-graph-end %%