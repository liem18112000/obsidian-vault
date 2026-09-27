---
ai_hash: f0faf494757a6d20
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities:
- Testing Agent
- Pipeline Stage
- Package
- Knowledge Gathering
- Test Plan Definition
- Execution
- Monolith
- A2A Agent
- server.py
- executor/ router
- A2A->MCP Bridge
- Claude
- GCS Memory Bank
- JSON Sidecar
- Rendered Markdown
- Claude-on-Vertex
- Heuristic Split
- knowledge_gathering.llm
- vertex_config()
- Resumable Multi-Turn Interrogation Loop
- A2A input-required
- Generate
- Reconfirm
- Gather
- Refine
- Business
- Technical
- QA
- Define
- Methodology
- Scope
- Metrics
- TestPlan
- Implement
- Test Data
- Happy Scenarios
- Negative Scenarios
- Steps
- Question
- Answer
- Insight
- Graph
- Pack
- MemoryBank
- llm.vertex.vertex_config
- RefineSession
- Approved Insight Pack
- context_id
- Approve Step
- Shared Bank
- Scenario
- Source Note
- JetBrains Excalidraw plugin
- .excalidraw source field
source: session 2026-08-28, test-plan-definition implementation plan
status: seedling
tags:
- test-agent
- a2a
- mcp
- agent-architecture
- design-decision
title: Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering
  skeleton
type: model
---

# Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton

The Testing Agent pipeline (knowledge_gathering -> test_plan_definition -> execution -> ...) is built so that **each stage is its own package that mirrors the `knowledge_gathering` skeleton**, rather than one monolith. The shared skeleton is: an **A2A agent** (`server.py` + `executor/` router) fronted by an **A2A->MCP bridge** so local Claude drives it, a **shared GCS Memory Bank** (JSON sidecar canonical + rendered Markdown), a **Claude-on-Vertex vs heuristic split** (now centralized in `knowledge_gathering.llm` via `vertex_config()`), and a **resumable, multi-turn interrogation loop** that pauses in A2A `input-required`.

**Key insight — same building blocks, reordered.** `knowledge_gathering` does *generate -> reconfirm*: `gather` (one-shot crawl) then `refine` (multi-turn reconfirm, rounds business/technical/qa). The next package `test_plan_definition` does *reconfirm -> generate*: `define` (multi-turn reconfirm, rounds **methodology/scope/metrics** -> a confirmed `TestPlan`) then `implement` (one-shot generation of test data + happy/negative scenarios + steps). Same two primitives (reconfirm loop + one-shot generator), flipped order.

**Reuse, do not duplicate.** The new package depends on `knowledge_gathering` for the generic contracts Question/Answer/Insight/Graph/Pack/MemoryBank and `llm.vertex.vertex_config`. `define` is essentially `RefineSession` with different round names + prompts. The **hand-off between stages is the approved insight pack, keyed by `context_id`** (produced by the `approve` step), read from the shared bank.

**Why:** one skeleton to learn/debug, warm-start persistence in the shared bank, and provenance edges linking scenario -> insight -> source note across stages.

See also [[JetBrains Excalidraw plugin rewrites the .excalidraw source field on save]].

## Related

- [[JetBrains Excalidraw plugin rewrites the .excalidraw source field on save]]

%% ai-graph-start %%

**Related notes:**
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]
- [[approve_plan is an agent-side write, unlike knowledge_gathering's read-only approve]]
- [[test-agent two A2A agents share a skeleton but diverge in domain engines]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]

**Relations:**
- Testing Agent — *builds* — Pipeline Stage
- Pipeline Stage — *is a* — Package
- Pipeline Stage — *mirrors skeleton of* — Knowledge Gathering
- Testing Agent — *avoids* — Monolith
- Testing Agent — *pipeline includes stage* — Knowledge Gathering
- Testing Agent — *pipeline includes stage* — Test Plan Definition
- Testing Agent — *pipeline includes stage* — Execution
- Knowledge Gathering — *skeleton includes* — A2A Agent
- A2A Agent — *comprises* — server.py
- A2A Agent — *comprises* — executor/ router
- A2A Agent — *fronted by* — A2A->MCP Bridge
- A2A->MCP Bridge — *driven by* — Claude
- Knowledge Gathering — *skeleton includes* — GCS Memory Bank
- GCS Memory Bank — *stores* — JSON Sidecar
- GCS Memory Bank — *stores* — Rendered Markdown
- Knowledge Gathering — *skeleton includes* — Claude-on-Vertex
- Knowledge Gathering — *skeleton includes* — Heuristic Split
- Claude-on-Vertex — *centralized in* — knowledge_gathering.llm
- Heuristic Split — *centralized in* — knowledge_gathering.llm
- knowledge_gathering.llm — *uses* — vertex_config()
- Knowledge Gathering — *skeleton includes* — Resumable Multi-Turn Interrogation Loop
- Resumable Multi-Turn Interrogation Loop — *pauses at* — A2A input-required
- Knowledge Gathering — *follows pattern* — Generate
- Knowledge Gathering — *follows pattern* — Reconfirm
- Knowledge Gathering — *performs* — Gather
- Knowledge Gathering — *performs* — Refine
- Gather — *is a type of* — one-shot crawl
- Refine — *is a type of* — multi-turn reconfirm
- Refine — *includes round type* — Business
- Refine — *includes round type* — Technical
- Refine — *includes round type* — QA
- Test Plan Definition — *follows pattern* — Reconfirm
- Test Plan Definition — *follows pattern* — Generate
- Test Plan Definition — *performs* — Define
- Test Plan Definition — *performs* — Implement
- Define — *is a type of* — multi-turn reconfirm
- Define — *includes round type* — Methodology
- Define — *includes round type* — Scope
- Define — *includes round type* — Metrics
- Define — *produces* — TestPlan
- Implement — *is a type of* — one-shot generation
- Implement — *generates* — Test Data
- Implement — *generates* — Happy Scenarios
- Implement — *generates* — Negative Scenarios
- Implement — *generates* — Steps
- Package — *depends on* — Knowledge Gathering
- Knowledge Gathering — *provides contract* — Question
- Knowledge Gathering — *provides contract* — Answer
- Knowledge Gathering — *provides contract* — Insight
- Knowledge Gathering — *provides contract* — Graph
- Knowledge Gathering — *provides contract* — Pack
- Knowledge Gathering — *provides contract* — MemoryBank
- Package — *depends on* — llm.vertex.vertex_config
- Define — *is essentially* — RefineSession
- Pipeline Stage — *hands off* — Approved Insight Pack
- Approved Insight Pack — *keyed by* — context_id
- context_id — *produced by* — Approve Step
- Approved Insight Pack — *read from* — Shared Bank
- Shared Bank — *provides* — warm-start persistence
- Provenance Edges — *links* — Scenario
- Provenance Edges — *links* — Insight
- Provenance Edges — *links* — Source Note
- Testing Agent — *related to* — JetBrains Excalidraw plugin

%% ai-graph-end %%