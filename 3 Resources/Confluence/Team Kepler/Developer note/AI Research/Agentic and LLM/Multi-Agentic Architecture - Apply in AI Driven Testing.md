---
ai_hash: 2abb7a526109ea32
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49259708519'
confluence_path: Team Kepler > Developer note > AI Research > Agentic and LLM
created: 2026-03-23
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
- search
title: 'Multi-Agentic Architecture: Apply in AI Driven Testing'
type: source
updated: 2026-03-23
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49259708519/Multi-Agentic+Architecture+Apply+in+AI+Driven+Testing
---

# Multi-Agentic Architecture: Apply in AI Driven Testing

*Confluence source · Team Kepler › Developer note › AI Research › Agentic and LLM · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49259708519/Multi-Agentic+Architecture+Apply+in+AI+Driven+Testing) · updated 2026-03-23*

## Overview

- Our AI testing framework (`helpers/ai/`) implements a multi-agent system built on the **ReAct pattern** with **Gemini function calling**.

- It automates the full lifecycle: from reading a Jira ticket, through test generation and step implementation, to creating a pull request — all driven by specialized AI agents.

## System Architecture

![[image-20260323-021434.png]]

## Agent Inventory

|  |  |  |  |  |
|----|----|----|----|----|
| Agent | Location | Iterations | Tools | Role |
| **TestGenerator** | `agents/test_generator/` | 15 | 7 | Generate Cucumber `.feature` files from Jira tickets, import to Xray |
| **StepImplementer** | `agents/step_implementer/` | 25 | 8 | Generate Python step definitions, run `behave` tests, iterate until pass |
| **PRCreator** | `agents/pr_creator/` | 10 | 4 | Create branch, commit files, push, create BitBucket PR |
| **RepoExplorer** | `agents/repo_explorer/` | 100 | 8 | Browse repos via REST API, build graph, store findings to Memory |
| **CodeAnalyzer** | `agents/code_analyzer/` | 25 | 8 | Clone repo, build graph, blast-radius analysis, change planning |
| **RequirementMemory** | `agents/requirement_memory/` | 15 | 3 | Extract requirements from Jira/Confluence, save to Memory Bank |
| **MemoryQuery** | `agents/memory_query/` | 10 | 2 | Answer questions from stored knowledge |
| **Orchestrator** | `agents/orchestrator.py` | — | — | Coordinates the full multi-phase flow (not a ReAct agent itself) |

## The ReAct Loop — Our Implementation

Every agent extends `BaseAgent` which implements the ReAct loop in `agents/base.py`

**Key implementation details:**

- **Retry**: 3 retries with 5s exponential backoff on transient errors (`base.py:255`)

- **Observation cap**: 30KB max per tool response to prevent context overflow (`base.py:248`)

- **Memory augmentation**: System prompt enriched with relevant facts from Memory Bank (`base.py:57`)

- **Token tracking**: Every API call records input/output/cached tokens for cost accounting (`base.py:112`)

![[image-20260323-021537.png]]

## Orchestrator Agent

The `OrchestratorAgent` (`agents/orchestrator.py`) is the **top-level coordinator** that drives the entire multi-agent pipeline.

Unlike the other agents, it does **not** use the ReAct loop — it is a deterministic Python class that calls sub-agents in sequence, manages shared infrastructure (cache, memory, cleanup), and handles failures gracefully.

|  |  |  |
|----|----|----|
| Aspect | Orchestrator Agent | Other Agents |
| **Pattern** | Deterministic Python code — sequential phases | ReAct loop — LLM decides what to do next |
| **LLM calls** | None — it does not call Gemini directly | Multiple calls per iteration via `generate_content` |
| **Decision making** | Hardcoded `if/else` | LLM chooses which tools to call based on observations |
| **Tools** | None — delegates to sub-agents instead | 7-8 tools per agent (registered via `build_tools()`) |
| **Role** | Infrastructure manager — cache, memory, cleanup, error handling | Domain specialist — generates tests, implements steps, creates PRs |

The orchestrator wraps optional steps so that failures don't crash the pipeline:

|  |  |  |
|----|----|----|
| Step | Error Handling | Impact of Failure |
| Memory warm-up | `try/except` → continue without memory | Agents work, just no cross-session learning |
| Requirement extraction | `try/except` → print warning | Test generation proceeds without extracted requirements |
| Test generation | **No catch** — failure propagates | Run aborts (can't proceed without features) |
| Step implementation | Conditional skip if no `test_set_key` | PR created with features only |
| PR creation | **No catch** — failure propagates | Result returned without `pr_url` |
| Memory save | Background thread, daemon=True | Thread killed on exit if still running |
| All cleanup | `finally` block | Always runs regardless of success/failure |

## Shared Context Flow

The `shared_ctx` dictionary accumulates state as it flows through agents:

![[image-20260323-021849.png]]

## Tool System

Each agent registers its tools via a `build_tools()` function that returns `list[ToolDef]`:

|  |  |  |  |
|----|----|----|----|
| TestGeneratorAgent | StepImplementerAgent | CodeAnalyzerAgent | RepoExplorerAgent |
| `fetch_jira_ticket` | `fetch_test_set` | `clone_repo` | `browse_repo_tree` |
| `save_feature_file` | `save_step_file` | `build_graph` | `browse_repo` |
| `clone_linked_repo` | `run_behave_tests` | `graph_stats` | `read_repo_file` |
| `import_to_xray` | `update_test_status` | `query_graph` | `search_code` |
| `check_existing_ai_tests` | `read_feature_files` | `impact_analysis` | `analyze_graph` |
| `get_codebase_context` | `get_codebase_context` | `search_graph` | `query_graph` |
| `read_cloned_file` | `read_cloned_file` | `read_file` | `note_finding` |
|  | `list_cloned_files` | `list_files` | `finish_repo` |

**ToolDef anatomy** (`agents/shared_tools.py`):

```
@dataclass
class ToolDef:
    name: str                    # Identifier for Gemini function calling
    description: str             # Natural language — guides LLM tool selection
    parameters: types.Schema     # JSON schema for argument validation
    handler: Callable            # Sync or async function (auto-wrapped)

    async def execute(args, shared_ctx):
        # Injects shared_ctx automatically into every handler
```

## Code Knowledge Graph — CodeAnalyzerAgent

The `CodeAnalyzerAgent` (`agents/code_analyzer/`) clones a repo, builds a knowledge graph, and performs blast-radius analysis:

![[image-20260323-023805.png]]

**Graph node types**: `File`, `Class`, `Function`, `Type`, `Test`, `Step`, `Feature`, `Scenario`, `Service`

**Graph edge types**: `CALLS`, `IMPORTS_FROM`, `CONTAINS`, `INHERITS`, `IMPLEMENTS`, `TESTED_BY`, `IMPLEMENTS_STEP`, `USES_SERVICE`

**Impact radius** uses BFS traversal from seed nodes (changed files) through the adjacency graph, finding all nodes within N hops. The result includes impacted files, untested functions, and guidance notes.

## Swarm Mode — Parallel Repository Exploration

The `explore_swarm()` function in `agents/repo_explorer/` enables concurrent exploration of multiple repositories:

![[image-20260323-023851.png]]

**How it works:**

1.  Resolve repos from slug, prefix (e.g., `luz-`), or comma-separated list

2.  Single Memory Bank warm-up (shared across all agents)

3.  `asyncio.Semaphore(max_concurrent)` limits parallelism

4.  Each agent independently browses, reads, graphs, and stores findings

5.  Memory Bank acts as a "pheromone trail" — Agent 2 can find Agent 1's discoveries immediately (see this [section](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49259708519/Multi-Agentic+Architecture+Apply+in+AI+Driven+Testing#Memory-Bank-as-%22Pheromone-Trail%22))

## Memory Bank — Cross-Session Learning

Memory Bank is a managed service within the Vertex AI Agent Engine that enables agents to **remember, learn, and adapt** across conversations. It transforms stateless agent interactions into stateful, personalized experiences.

**Lifecycle management**

- **TTL Expiration** — Configure automatic expiration so stale memories are deleted after a set duration.

- **Memory Revisions** — Inspect how memories transform as new information is ingested over time.

- **IAM Permissions** — Restrict which principals can read or write specific scopes' memories.

- **Model Armor** — Inspect prompts to mitigate memory poisoning attacks.

![[image-20260323-033631.png]]

### Phase 1 — Session interaction

When a conversation begins, the system creates a **Session** via `CreateSession`. Each session is a chronological sequence of messages between a user and an agent, and every session must have a **user ID** to map extracted memories to that specific user.

![[image-20260323-032008.png]]

As the user interacts with the agent, events (user messages, agent responses, tool actions) are uploaded to the Session Store via `AppendEvent`. These events persist the conversation history and create the raw material for memory generation.

### Phase 2 — Memory generation (asynchronous)

This is where the intelligence lives. Memory generation runs **asynchronously in the background**, adding no latency to the user experience. It consists of two LLM-powered steps.

![[image-20260323-032045.png]]

#### Step A — Extraction

A Gemini model analyzes the raw conversation transcript and extracts key facts, preferences, and context. The extraction is guided by **Memory Topics** — configurable labels that tell Memory Bank what information to persist.

**Default managed topics include:**

|  |  |  |
|----|----|----|
| Topic | Description | Example |
| `USER_PERSONAL_INFO` | Names, relationships, hobbies, dates | "My wedding anniversary is Dec 31" |
| `USER_PREFERENCES` | Likes, dislikes, preferred methods | "I prefer aisle seats on flights" |
| `EXPLICIT_INSTRUCTIONS` | Direct instructions for future behavior | "Always book gluten-free meals" |
| `KEY_CONVERSATION_DETAILS` | Important facts from conversations | "User is planning a trip to Hawaii" |

You can also define **custom topics** with few-shot examples to teach Memory Bank what to extract for your specific domain.

#### Step B — Consolidation

Consolidation is the intelligence layer that prevents the memory store from becoming a mess of duplicates and contradictions. Memory Bank checks new memories against existing ones with the **same scope** and makes decisions:

### Phase 3 — Memory retrieval

When a new session begins, the agent can retrieve memories about the user. Two retrieval modes are available:

ADK provides two built-in retrieval tools:

- `PreloadMemory` — Always retrieves memories at the beginning of each agent turn (similar to a callback).

- `LoadMemory` — Retrieves memories on-demand when the agent decides past context would be helpful.

### How we apply Memory Bank

The Memory Bank enables agents to **learn across sessions** — facts discovered in Run N are available in Run N+1. It is backed by **Vertex AI Agent Engine** and uses semantic search (embedding model `text-embedding-005`) for retrieval.

![[image-20260323-034732.png]]![[image-20260323-023947.png]]

### Memory Bank as "Pheromone Trail"

The Memory Bank is the key inter-agent communication channel within a swarm

1.  All swarm agents share the same `MemoryService` singleton

2.  `memory.create()` writes immediately to Vertex AI Agent Engine

3.  `memory.retrieve()` uses semantic search — no exact key needed

4.  A fact stored by Agent 1 at time T is retrievable by Agent 3 at time T+1

![[image-20260323-034012.png]]

### Scope Hierarchy

Every memory is stored under a **hierarchical scope** that prevents cross-contamination:

```
Format: agent-name / [repo-slug /] project-key
```

|  |  |
|----|----|
| Scope | Meaning |
| `test-generator/LUZ` | TestGenerator facts for project LUZ (no specific repo) |
| `repo-explorer/luz-docs/LUZ` | RepoExplorer facts for luz-docs repo in project LUZ |
| `repo-explorer/luz-auth/LUZ` | RepoExplorer facts for luz-auth repo in project LUZ |
| `step-implementer/LUZ` | StepImplementer facts for project LUZ |
| `orchestrator/LUZ` | Orchestrator-level facts (success/failure summaries) |

Agents only retrieve from their own scope — `test-generator` never sees `repo-explorer` facts.

### How Memories Are Injected into Agents

In `BaseAgent._build_system_prompt()` (`agents/base.py:57-72`):

1.  Use the **task text** as a semantic search query

2.  Call `memory.retrieve(agent_name, task, top_k=10)` → returns up to 10 similar facts

3.  Check **in-process cache** (SHA256 of agent+query+top_k) → skip API if already retrieved this run

4.  Format facts as bullet list

5.  Append to system prompt:

```
[Original system prompt...]
## Relevant memories from previous sessions
- [luz-docs] Folder creation requires parentFolderIds as array
- [luz-docs] Security classes are validated against tenant's SC list
- LUZ-12345 test generation succeeded with 3 feature files
```

The LLM sees these memories as part of its instructions and uses them to make better decisions.

## Token Tracking & Cost Accounting

The token tracker makes this visible, allowing teams to:

- **Monitor costs** per agent and per run

- **Identify expensive agents** (StepImplementer often uses the most iterations)

- **Measure cache effectiveness** (cached vs non-cached ratio)

- **Budget forecasting** by analyzing exported JSON reports over time

### How It Works

|  |  |  |
|----|----|----|
| Field from Gemini | Mapped to | Meaning |
| `prompt_token_count` | `input_tokens` | Total input tokens (includes cached) |
| `candidates_token_count` | `output_tokens` | Generated output tokens |
| `cached_content_token_count` | `cached_tokens` | Portion of input that hit context cache |

![[image-20260323-035538.png]]

```
Cost = (input_tokens - cached_tokens) × input_price / 1M + cached_tokens × cached_price / 1M + output_tokens × output_price / 1M
```

```
======================================================================
TOKEN USAGE SUMMARY  (model: gemini-2.5-pro)
======================================================================
Agent                 Calls      Input     Output     Cached      Cost
----------------------------------------------------------------------
TestGenerator            8    178,330      3,237    106,110 $ 0.1558
StepImplementer         12     95,400      8,921     72,300 $ 0.0834
PRCreator                3     12,100      1,450      9,200 $ 0.0092
----------------------------------------------------------------------
TOTAL                   23    285,830     13,608    187,610 $ 0.2484
======================================================================
  Input tokens:     285,830  (non-cached: 98,220, cached: 187,610)
  Output tokens:     13,608
  Total tokens:     299,438
  Estimated cost: $0.2484
```

## Context Caching — Cost Optimization

In a multi-agent run, every agent needs the same codebase context (features, steps, services). Without caching, this context is sent as fresh input tokens on **every API call** across every agent iteration. Context caching uploads this shared context **once** to Gemini, then all subsequent calls reference the cached version at a **75% discount**.

![[image-20260323-024043.png]]

|  |  |  |  |
|----|----|----|----|
| Content | Source | Limit | Purpose |
| **Feature file samples** | `features/**/*.feature` | 3 files, 100 lines each | Show agents existing test patterns |
| **Feature conventions** | `prompts/templates/feature_conventions.md` | Full template | Rules for writing new features |
| **Available step definitions** | `@given/@when/@then` decorators in `features/steps/*.py` | All steps | Reuse existing steps, avoid duplicates |
| **Step implementation samples** | `features/steps/*.py` | 3 files, 150 lines each | Show coding patterns for step code |
| **Step conventions** | `prompts/templates/step_conventions.md` | Full template | Rules for implementing steps |
| **Service signatures** | `def` signatures in `core/service/*.py` | All signatures | Available API wrappers to call |
| **Test data files** | `resources/test-data/*.json` | File names only | Available test data |

If cache creation fails, agents still work — they just pay full price:

|  |  |
|----|----|
| Scenario | Impact |
| Cache creation succeeds | All agents get 75% discount on cached tokens |
| Cache creation fails (API error) | Agents work normally at full price |
| Context too small (\< 4096 chars) | Cache skipped, full price |
| Cache deletion fails | TTL (1 hour) expires it automatically |

![[image-20260323-040720.png]]

## Prefetch System — Parallel Data Gathering

The prefetch system (`prefetch.py`) runs **all data-gathering tasks concurrently** before any agent starts its ReAct loop. This eliminates sequential API wait times and produces a rich task prompt that lets agents skip their own info-gathering tool calls.

|  |  |
|----|----|
| Source | What It Does |
| Jira REST API | Fetches ticket fields (summary, description, acceptance criteria, labels, components). Parses Atlassian Document Format (ADF) to plain text. |
| Jira REST API | Fetches all comments on the ticket. Formats as chronological thread with author and timestamp. |
| Jira REST API | Downloads each attachment. Text files → plain text. Images → base64 encoding. Binary → placeholder. |
| Confluence REST API | Discovers linked Confluence pages from the Jira ticket. Fetches and formats page content. |
| Git + Bitbucket | Finds the linked Bitbucket branch from the Jira ticket. Clones it `--depth 1` into a temp directory. |
| Local file system | Scans the local project for existing features, step definitions, service signatures, test data files, and conventions. |

![[image-20260323-040327.png]]

## File Structure

```
helpers/ai/
├── agents/
│   ├── base.py                  # BaseAgent + ReAct loop + context cache
│   ├── orchestrator.py          # Multi-phase coordinator
│   ├── runner.py                # CLI entry points (full, generate, implement, explore, analyze)
│   ├── shared_tools.py          # ToolDef + schema helpers + shared handlers
│   ├── code_analyzer/           # Blast-radius analysis agent
│   │   ├── agent.py             #   CodeAnalyzerAgent + analyze_code()
│   │   └── tools.py             #   clone, graph, query, impact tools
│   ├── test_generator/          # Feature file generation
│   ├── step_implementer/        # Step code generation + test runner
│   ├── pr_creator/              # BitBucket PR creation
│   ├── repo_explorer/           # Repository exploration + swarm
│   ├── requirement_memory/      # Requirement extraction
│   └── memory_query/            # Knowledge base Q&A
├── graphs/
│   ├── builder.py               # Full/incremental graph builds
│   ├── parser.py                # Tree-sitter code parsing
│   ├── query.py                 # Named patterns + impact radius
│   ├── store.py                 # SQLite graph storage
│   ├── models.py                # GraphNode, GraphEdge, NodeKind, EdgeKind
│   ├── embeddings.py            # Vector storage + semantic search
│   └── firestore_store.py       # Firestore backend
├── memory/
│   ├── service.py               # Memory Bank REST API wrapper
│   ├── config.py                # Scope building, topics, Agent Engine config
│   └── setup.py                 # Agent Engine initialization
├── clients/
│   ├── gemini/client.py         # Vertex AI Gemini singleton
│   └── google_auth/client.py    # OAuth2 token management
├── contexts/
│   ├── builder.py               # Codebase context collection
│   └── samples.py               # Feature/step sample extraction
├── prompts/
│   ├── __init__.py              # load_prompt() from templates/
│   └── templates/               # Markdown prompt templates
├── token/
│   └── tracker.py               # Token usage + cost tracking
├── costs/
│   ├── exporter.py              # Cost report generation
│   └── pricing.py               # Model pricing lookup
├── bitbucket.py                 # Git commands + BitBucket PR API
├── jira_reader.py               # Jira ticket/attachment reading
├── prefetch.py                  # Parallel context gathering
├── common.py                    # File I/O + display helpers
└── service.py                   # CLI service entry point
```

## CLI Usage

```
# Full multi-agent pipeline: generate + implement + PR
python -m helpers.ai.agents.runner full LUZ-149716

# Generate feature files only
python -m helpers.ai.agents.runner generate LUZ-149716

# Implement step definitions only
python -m helpers.ai.agents.runner implement LUZ-150653

# Explore repositories (single, prefix, or list)
python -m helpers.ai.agents.runner explore luz-docs
python -m helpers.ai.agents.runner explore luz-
python -m helpers.ai.agents.runner explore luz-docs,luz-auth

# Blast-radius analysis / change planning
python -m helpers.ai.agents.runner analyze luz-docs "Add delete endpoint for folders"
python -m helpers.ai.agents.runner analyze luz-docs:develop "Refactor auth middleware"

# Extract requirements to Memory Bank
python -m helpers.ai.agents.runner requirements LUZ-149716

# Query stored knowledge
python -m helpers.ai.agents.runner memory "What API endpoints does luz-docs have?"
```

%% ai-graph-start %%

**Related notes:**
- [[Multi-Agentic Architecture - Theory]]
- [[Test-Plan Definition Agent]]
- [[Code Knowledge Graph - Apply in AI Test Driven]]
- [[Agent Loop 1 - Knowledge Gathering - v2]]
- [[Test Executor Agent - Closing the Testing Pipeline Gap]]

%% ai-graph-end %%