---
ai_hash: c8dfdaf14d1da7de
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities:
- Pipeline stages
- context_id
- Testing Agent
- GCS memory bank
- knowledge_gathering
- refine stage
- memory/refine/<ctx>/
- questions.json
- answers.json
- state.json
- understanding.md
- test_plan_definition
- define stage
- memory/test-plan/<ctx>/
- plan.json
- decisions.json
- plan-brief.md
- PlanSession
- RefineSession
- Insight
- understanding artifacts
- ingest
- generate_round
- accept_recommendation
- state machine
- test-plan/ writers
- PlanDecision
- TestPlan
- stateless helpers
- stateful driver
- closure generator
- Pack
- (Pack, round)->questions ranker/capper
- (Pack, str) signature
- Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering
  skeleton
source: session 2026-08-28, test_plan_definition M1
status: seedling
tags:
- test-agent
- memory-bank
- gcs
- gotcha
- design-decision
title: Pipeline stages sharing a context_id need separate memory-bank path prefixes
type: lesson
---

# Pipeline stages sharing a context_id need separate memory-bank path prefixes

In the Testing Agent, a single `context_id` (e.g. `run-6f2a`) flows through multiple stages that all read/write the **same** GCS memory bank. `knowledge_gathering`'s refine stage persists under `memory/refine/<ctx>/` (questions.json, answers.json, state.json, understanding.md). If `test_plan_definition`'s define stage — which is a refine-shaped loop on the SAME `context_id` — reused those same paths, its questions/state would silently **clobber** the refine run's.

**Fix:** give each stage its own path prefix. Define writes under `memory/test-plan/<ctx>/` (plan.json, questions.json, answers.json, decisions.json, state.json, plan-brief.md), never the refine paths. Same rule for any future stage.

**Design consequence:** `PlanSession` does NOT reuse `RefineSession` wholesale even though it is structurally a refine loop — because RefineSession's persistence is hardwired to the refine/ paths and emits `Insight`/understanding artifacts. Instead it reuses the *generic, persistence-free* helpers (`ingest`, `generate_round`, `accept_recommendation`) and owns its own state machine + `test-plan/` writers, emitting `PlanDecision`/`TestPlan`. Reuse the stateless helpers; duplicate the stateful driver when its persistence/artifacts differ.

Related technique: reuse `refine.generate_round` (a generic `(Pack, round)->questions` ranker/capper) from another stage by passing a **closure generator** that carries extra stage context (the confirmed understanding) while keeping the `(Pack, str)` signature it expects.

## Related

- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]

%% ai-graph-start %%

**Related notes:**
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[test-agent-v2 executor tests share memory-bank state and fail by test order]]
- [[Testing-Agent GCS memory bank one bucket, memory root, five subfolders]]
- [[test-agent-v2 test_evaluation restructure engine + config packages + merged golden set]]
- [[Add cross-stage provenance to a shared graph via update_index without new model methods]]

**Relations:**
- Pipeline stages — *share* — context_id
- Pipeline stages — *need* — separate memory-bank path prefixes
- Testing Agent — *uses* — context_id
- context_id — *flows through* — Pipeline stages
- Pipeline stages — *read/write* — GCS memory bank
- knowledge_gathering — *has* — refine stage
- refine stage — *persists under* — memory/refine/<ctx>/
- memory/refine/<ctx>/ — *stores* — questions.json
- memory/refine/<ctx>/ — *stores* — answers.json
- memory/refine/<ctx>/ — *stores* — state.json
- memory/refine/<ctx>/ — *stores* — understanding.md
- test_plan_definition — *has* — define stage
- define stage — *is a* — refine-shaped loop
- define stage — *uses* — context_id
- define stage — *would clobber* — refine stage
- define stage — *writes under* — memory/test-plan/<ctx>/
- memory/test-plan/<ctx>/ — *stores* — plan.json
- memory/test-plan/<ctx>/ — *stores* — questions.json
- memory/test-plan/<ctx>/ — *stores* — answers.json
- memory/test-plan/<ctx>/ — *stores* — decisions.json
- memory/test-plan/<ctx>/ — *stores* — state.json
- memory/test-plan/<ctx>/ — *stores* — plan-brief.md
- PlanSession — *does not reuse* — RefineSession
- RefineSession — *persistence is hardwired to* — refine/ paths
- RefineSession — *emits* — Insight
- RefineSession — *emits* — understanding artifacts
- PlanSession — *reuses* — ingest
- PlanSession — *reuses* — generate_round
- PlanSession — *reuses* — accept_recommendation
- PlanSession — *owns* — state machine
- PlanSession — *owns* — test-plan/ writers
- PlanSession — *emits* — PlanDecision
- PlanSession — *emits* — TestPlan
- stateless helpers — *should be* — reused
- stateful driver — *should be* — duplicated
- refine.generate_round — *is a* — (Pack, round)->questions ranker/capper
- generate_round — *can be reused by* — another stage
- generate_round — *expects* — (Pack, str) signature
- closure generator — *carries* — extra stage context
- closure generator — *maintains* — (Pack, str) signature
- Pipeline stages sharing a context_id need separate memory-bank path prefixes — *related to* — Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton

%% ai-graph-end %%