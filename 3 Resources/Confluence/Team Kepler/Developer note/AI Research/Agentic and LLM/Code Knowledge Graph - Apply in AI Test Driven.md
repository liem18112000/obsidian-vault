---
title: "Code Knowledge Graph: Apply in AI Test Driven"
created: 2026-03-25
updated: 2026-03-25
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49269506102/Code+Knowledge+Graph+Apply+in+AI+Test+Driven
confluence_id: "49269506102"
confluence_path: "Team Kepler > Developer note > AI Research > Agentic and LLM"
tags: [confluence, ai-agents, search]
---

# Code Knowledge Graph: Apply in AI Test Driven

*Confluence source · Team Kepler › Developer note › AI Research › Agentic and LLM · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49269506102/Code+Knowledge+Graph+Apply+in+AI+Test+Driven) · updated 2026-03-25*

## Executive Summary

The Code Knowledge Graph transforms raw source code into a queryable structural database that AI agents use for blast-radius analysis, test coverage detection, and semantic code search. This document covers the full pipeline — from parsing to querying — across 12 sections.

|  |  |
|----|----|
| **Aspect** | **Detail** |
| **What it is** | A directed graph where nodes are code entities (files, classes, functions, BDD steps/scenarios) and edges are relationships (calls, imports, contains, inherits, tested-by). |
| **How it's built** | `CodeParser` uses tree-sitter ASTs for Python/Java/JS/TS/Go and regex for Gherkin `.feature` files. `GraphBuilder` orchestrates full or incremental builds with SHA-256 hash-based skip to avoid re-parsing unchanged files. |
| **Where it's stored** | Dual backends — **SQLite** (local, WAL mode, supports embeddings) or **Firestore** (serverless, REST API, free-tier friendly). Both expose the same API. |
| **How it's queried** | `GraphQuery` provides 8 named patterns (`callers_of`, `tests_for`, `inheritors_of`, etc.), bidirectional BFS impact-radius analysis (configurable depth + cap), and Vertex AI embedding-powered semantic search with cosine similarity. |
| **Who uses it** | `RepoExplorerAgent` builds the graph while reading remote files and queries it for structural insights. `CodeAnalyzerAgent` clones a repo, builds the graph, and runs impact analysis for change planning. The code-review-graph MCP plugin exposes it to external tools. |
| **BDD integration** | First-class support for Gherkin features/scenarios and Python step definitions (`@given`/`@when`/`@then`), linked via `IMPLEMENTS_STEP` edges. `find_untested_functions()` identifies functions with no `TESTED_BY` coverage. |
| **Key optimisation** | Incremental builds parse only changed files + their dependents (via `git diff` + import-edge tracing). Adjacency lists for BFS are cached in-memory and invalidated on write. Embedding generation uses hash-based skip and batched Vertex AI calls. |

## Overview

The module implements a **code knowledge graph** — a structural representation of source code as nodes (code entities) and edges (relationships). It powers blast-radius analysis, test coverage detection, and semantic code search across the AI agent system.

![[image-20260325-034621.png]]

## Module Structure

```
helpers/ai/graphs/
├── __init__.py             # Public API exports
├── models.py               # Data classes: GraphNode, GraphEdge, NodeKind, EdgeKind
├── parser.py               # Tree-sitter + Gherkin parser → nodes & edges
├── builder.py              # Full/incremental build orchestrator
├── store.py                # SQLite-backed graph storage (BFS, keyword search)
├── firestore_store.py      # Firestore-backed graph storage (REST API)
├── query.py                # Named query patterns + impact radius + review context
├── embeddings.py           # Vertex AI embeddings + cosine similarity search
├── verify_firestore.py     # Firestore connectivity verification
└── schemas/
    ├── __init__.py          # Schema loader
    ├── nodes.sql            # CREATE TABLE nodes
    ├── edges.sql            # CREATE TABLE edges
    ├── metadata.sql         # CREATE TABLE metadata
    └── embeddings.sql       # CREATE TABLE embeddings
```

## 1. Data Model (`models.py`)

### Node Kinds

Nodes represent code entities extracted from source files.

|  |  |  |
|----|----|----|
| Kind | Description | Source |
| `File` | Source file (one per parsed file) | All languages |
| `Class` | Class, interface, enum, type alias | Python, Java, JS/TS, Go |
| `Function` | Function or method | All languages |
| `Type` | Type alias or declaration | TypeScript, Go |
| `Test` | Test function (detected by naming pattern) | All languages |
| `Step` | BDD step definition (`@given`/`@when`/`@then`) | Python (behave) |
| `Feature` | Gherkin feature | `.feature` files |
| `Scenario` | Gherkin scenario or scenario outline | `.feature` files |
| `Service` | API service class/module | Domain-specific |

### Edge Kinds

Edges represent relationships between nodes.

![[image-20260325-034728.png]]

|  |  |  |
|----|----|----|
| Edge Kind | Direction | Meaning |
| `CALLS` | caller → callee | Function invokes another function |
| `IMPORTS_FROM` | file → module | File imports from module |
| `CONTAINS` | parent → child | File contains class, class contains method |
| `INHERITS` | subclass → superclass | Class extends another class |
| `IMPLEMENTS` | class → interface | Class implements interface |
| `TESTED_BY` | target → test | Function is tested by a test function |
| `DEPENDS_ON` | A → B | General dependency |
| `IMPLEMENTS_STEP` | step def → gherkin step | Step definition maps to scenario step |
| `USES_SERVICE` | function → service | Function calls a service API |

## 2. Parser (`parser.py`)

The `CodeParser` extracts nodes and edges from source files using two strategies:

**Supported Languages**

|  |  |  |  |
|----|----|----|----|
| Language | Extensions | Parser | Extracts |
| Python | `.py` | tree-sitter | classes, functions, imports, calls, step defs |
| Java | `.java` | tree-sitter | classes, interfaces, enums, methods, imports |
| JavaScript | `.js`, `.jsx` | tree-sitter | classes, functions, arrow functions, imports |
| TypeScript | `.ts`, `.tsx` | tree-sitter | classes, interfaces, types, functions, imports |
| Go | `.go` | tree-sitter | types, functions, methods, imports |
| Gherkin | `.feature` | regex | features, scenarios, step references |

![[image-20260325-034824.png]]

### Call Target Resolution

The parser resolves bare function calls to qualified names in three stages:

![[image-20260325-034914.png]]

### Test Detection

Functions are marked as tests based on naming patterns:

```
^test_       → test_create_user
^test[A-Z]   → testCreateUser
_test$       → create_user_test
_spec$       → payment_spec
^it_         → it_should_work
^should_     → should_return_200
```

Files matching test path patterns also mark all functions as tests:

```
test_*.py, *_test.py, *_test.go
*.test.tsx, *.spec.ts
/tests/, /__tests__/
```

## 3. Graph Builder (`builder.py`)

The `GraphBuilder` orchestrates full and incremental builds.

![[image-20260325-035038.png]]

#### Incremental Build

![[image-20260325-035106.png]]

### File Collection Strategy

**Ignored directories:** `node_modules`, `.git`, `__pycache__`, `.venv`, `dist`, `build`, `.tox`, `.gradle`, `.idea`, `target`, etc.

**Ignored extensions:** `.min.js`, `.lock`, `.pyc`, `.class`, `.jar`, images, fonts, archives, binaries, etc.

![[image-20260325-035141.png]]

## 4. Storage Backends

### 4.1 SQLite (`store.py`)

The primary local storage backend optimized for concurrent reads and fast BFS traversal.

![[image-20260325-035211.png]]

|  |  |
|----|----|
| Feature | Implementation |
| Concurrency | WAL journal mode + `busy_timeout=5000ms` |
| Atomicity | File-atomic upserts: DELETE old + INSERT new per file |
| BFS cache | In-memory adjacency lists (forward + reverse), invalidated on write |
| Thread safety | `threading.Lock` on adjacency cache |
| Variable limit | Batched IN-clause queries (batch size 450, SQLite limit 999) |
| Location | `.code-graph/graph.db` inside the repository |

### 4.2 Firestore (`firestore_store.py`)

Serverless cloud storage with the same public API as the SQLite store.

|  |  |
|----|----|
| Feature | Implementation |
| Authentication | Google OAuth2 via `helpers.ai.clients.google_auth` |
| Batch writes | Max 500 documents per `batchWrite` call |
| Keyword search | Client-side filtering (loads candidates, filters in Python) |
| BFS cache | Same in-memory adjacency strategy as SQLite |
| Costs | Free tier: 50K reads/day, 20K writes/day, 1 GiB |

## 5. Impact Radius Analysis (BFS)

The impact radius computes the "blast zone" of a code change — all nodes within N hops of the changed files.

![[image-20260325-035312.png]]

The BFS uses **cached adjacency lists** built from all edges in the graph:

```
# Forward: source → set of targets (who does this node call/depend on?)
# Reverse: target → set of sources (who calls/depends on this node?)
neighbors = forward.get(qn, set()) | reverse.get(qn, set())
```

**Example:** If `service.py::create_user` is changed:

- **Depth 0:** `create_user` itself

- **Depth 1:** `controller.py::handle_create` (calls it), `validator.py::validate_email` (called by it)

- **Depth 2:** `routes.py::register_routes` (calls the controller), `utils.py::normalize_email` (called by validator)

## 6. Query API (`query.py`)

#### Named Query Patterns

|  |  |  |  |
|----|----|----|----|
| Pattern | Edge Kind | Direction | Returns |
| `callers_of` | `CALLS` | reverse (target=node) | Functions that call the target |
| `callees_of` | `CALLS` | forward (source=node) | Functions called by the target |
| `imports_of` | `IMPORTS_FROM` | forward | Modules imported by the target |
| `importers_of` | `IMPORTS_FROM` | reverse | Files that import the target |
| `children_of` | `CONTAINS` | forward | Classes/functions inside the target |
| `tests_for` | `TESTED_BY` | forward | Tests that cover the target |
| `inheritors_of` | `INHERITS`/`IMPLEMENTS` | reverse | Subclasses/implementors |
| `file_summary` | all | both | All nodes and edges in the file |

### Target Resolution

When a query target is provided as a short name (e.g., `"create_user"`), it is resolved in order:

![[image-20260325-035443.png]]

### Review Context

`review_context()` combines multiple analyses into actionable code review guidance:

![[image-20260325-035511.png]]

## 7. Semantic Search (`embeddings.py`)

The embedding system adds vector search capability on top of the structural graph.

![[image-20260325-035548.png]]

### Search Tiers

![[image-20260325-035643.png]]

### Vector Storage

Vectors are stored as packed `float32` binary blobs in SQLite:

```
# Pack: list[float] → bytes
blob = struct.pack(f"{len(vector)}f", *vector)

# Unpack: bytes → list[float]
vector = list(struct.unpack(f"{n}f", blob))
```

## 8. BDD Integration

The graph has first-class support for Behaviour-Driven Development artifacts.

![[image-20260325-035731.png]]

## 9. How Agents Use the Graph

![[image-20260325-035812.png]]

## 10. Build Modes Comparison

|  |  |  |  |
|----|----|----|----|
| Mode | When to Use | Speed | Files Parsed |
| **Full** (`build()`) | First run or periodic refresh | Medium | All (skips unchanged via hash) |
| **Incremental** (`update()`) | After each commit | Fast | Changed + their dependents |
| **Force** (`build(force=True)`) | Parser upgrade or corruption | Slow | All (no skipping) |
