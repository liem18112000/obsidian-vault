---
ai_hash: b8b1678c44ff2870
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities:
- Cross-stage provenance
- Shared graph
- update_index
- Model methods
- Implement stage
- Test-plan nodes
- Test-scenario nodes
- Scenario -> insight/note edges
- Knowledge-index graph
- Gather stage
- Refine stage
- MemoryBank
- Callback
- Graph nodes dicts
- Graph edges dicts
- Graph.add_insight
- _add_provenance function
- Graph model
- Stage-specific vocabulary
- Queryable graph
- Compare-and-set retry
- Index-markdown re-render
- Mutate callback
- Node/edge dict shape
- Two packages
- Testing Agent
- Pipeline stage
- knowledge_gathering skeleton
source: session 2026-08-28, test_plan_definition M3
status: seedling
tags:
- test-agent
- memory-bank
- provenance
- graph
- design-decision
title: Add cross-stage provenance to a shared graph via update_index without new model
  methods
type: lesson
---

# Add cross-stage provenance to a shared graph via update_index without new model methods

The implement stage records cross-stage provenance (test-plan and test-scenario nodes, with edges scenario -> insight/note) into the SAME knowledge-index graph that gather/refine populate. It does this without adding `add_plan`/`add_scenario` methods to `knowledge_gathering.models.Graph`: the shared `MemoryBank.update_index(mutate)` runs a callback that writes directly into the graph's public `nodes` / `edges` dicts (the exact shape `Graph.add_insight` uses internally).

```python
bank.update_index(lambda g: _add_provenance(g, plan, scenarios))

def _add_provenance(graph, plan, scenarios):
    graph.nodes[plan.id] = {"id": plan.id, "type": TEST_PLAN, "title": ...}
    for ref in plan.source_refs:
        graph.edges[f"{plan.id}->{ref}"] = {"source_id": plan.id, "target": ref, ...}
    for sc in scenarios:
        graph.nodes[sc.id] = {"id": sc.id, "type": TEST_SCENARIO, "title": sc.title}
        for ref in sc.source_refs:
            graph.edges[f"{sc.id}->{ref}"] = {...}
```

**Why:** keeps the reusable `Graph` model free of stage-specific vocabulary (a new stage would otherwise keep bolting methods onto it), while still landing in one queryable graph. `update_index` already does the compare-and-set retry + index-markdown re-render, so the mutate callback is the right seam. Trade-off: the node/edge dict shape (`id/type/title`; `source_id/target/type/origin/in_scope`) is now an informal contract two packages depend on — keep it stable.

## Related

- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]

%% ai-graph-start %%

**Related notes:**
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]
- [[Write-stub bank proxy test a persistence-writing pipeline against live reads without mutating state]]
- [[approve_plan is an agent-side write, unlike knowledge_gathering's read-only approve]]
- [[test-agent-v2 executor step handlers take the executor as first arg]]

**Relations:**
- Cross-stage provenance — *added to* — Shared graph
- Cross-stage provenance — *added via* — update_index
- Cross-stage provenance — *added without* — Model methods
- Implement stage — *records* — Cross-stage provenance
- Cross-stage provenance — *includes* — Test-plan nodes
- Cross-stage provenance — *includes* — Test-scenario nodes
- Cross-stage provenance — *includes* — Scenario -> insight/note edges
- Implement stage — *records into* — Knowledge-index graph
- Gather stage — *populates* — Knowledge-index graph
- Refine stage — *populates* — Knowledge-index graph
- MemoryBank — *uses* — update_index
- update_index — *runs* — Callback
- Callback — *writes into* — Graph nodes dicts
- Callback — *writes into* — Graph edges dicts
- Graph nodes dicts — *has shape of* — Graph.add_insight
- Graph edges dicts — *has shape of* — Graph.add_insight
- update_index — *calls* — _add_provenance function
- _add_provenance function — *adds* — Test-plan nodes
- _add_provenance function — *adds* — Test-scenario nodes
- Graph model — *avoids* — Stage-specific vocabulary
- Cross-stage provenance — *stored in* — Queryable graph
- update_index — *performs* — Compare-and-set retry
- update_index — *performs* — Index-markdown re-render
- Mutate callback — *is a* — right seam
- Node/edge dict shape — *is contract for* — Two packages
- Testing Agent — *builds* — Pipeline stage
- Pipeline stage — *mirrors* — knowledge_gathering skeleton

%% ai-graph-end %%