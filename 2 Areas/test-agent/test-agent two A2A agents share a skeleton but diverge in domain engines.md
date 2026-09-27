---
ai_hash: 189a2a72f6b26a75
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-31
entities:
- test-agent monorepo
- A2A agents
- knowledge_gathering (KGA)
- test_plan_definition (TPD)
- identical skeleton
- domain engines
- '`__init__`'
- '`constants`'
- '`monitoring`'
- '`server`'
- '`bridge`'
- '`executor`'
- '`models`'
- framework/contract layer
- '`loop/`'
- crawl engine
- '`gather`'
- '`refine`'
- '`define/`'
- '`implement/`'
- '`llm/` (TPD local)'
- '`memory/`'
- '`render/`'
- gherkin
- '`define`'
- '`implement`'
- '`common/llm`'
- '`common`'
- Service symmetry belongs at the contract layer not the domain layer
source: session 2026-08-31
status: seedling
tags:
- test-agent
- a2a
- package-structure
- architecture
title: test-agent two A2A agents share a skeleton but diverge in domain engines
type: observation
---

# test-agent two A2A agents share a skeleton but diverge in domain engines

The test-agent monorepo has two A2A agents — **knowledge_gathering** (KGA, a read-only Atlassian crawler) and **test_plan_definition** (TPD, a test-plan author). They deliberately share an **identical skeleton** but **diverge in their domain engines**.

**Identical skeleton (the A2A-agent shape):** both have `__init__`, `constants`, `monitoring`, `server`, `bridge/{__init__,__main__,mcp_server}`, `executor/{__init__,base,common}`, and `models/`. This is the framework/contract layer every A2A agent needs — aligning it aids navigation.

**Divergent domain engines (by job):**
- KGA: `loop/` (seed -> fetch/ -> crawl) — one small crawl engine; executor handlers `gather`/`refine`.
- TPD: `define/` + `implement/` + `llm/` + `memory/` + `render/` (gherkin); executor handlers `define`/`implement`. More packages because it does more: interrogate -> plan -> generate testdata/scenarios/steps -> render.

**Tell-tale asymmetry that proves the principle:** KGA has *no local* `llm/` because its LLM work is generic (distill) and hoisted into `common/llm`; TPD keeps a local `llm/` because its prompts are domain-specific. Generic work migrated up, specific work stayed local.

Related invariant: `common` never imports an agent, and the two agents never import each other. This is the worked example behind [[Service symmetry belongs at the contract layer not the domain layer]].

## Related

- [[Service symmetry belongs at the contract layer not the domain layer]]

%% ai-graph-start %%

**Related notes:**
- [[Service symmetry belongs at the contract layer not the domain layer]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
- [[test-agent-v2 test_evaluation restructure engine + config packages + merged golden set]]

**Relations:**
- test-agent monorepo — *CONTAINS* — A2A agents
- A2A agents — *INCLUDE* — knowledge_gathering (KGA)
- A2A agents — *INCLUDE* — test_plan_definition (TPD)
- knowledge_gathering (KGA) — *IS_A* — A2A agent
- test_plan_definition (TPD) — *IS_A* — A2A agent
- knowledge_gathering (KGA) — *PERFORMS_ROLE* — read-only Atlassian crawler
- test_plan_definition (TPD) — *PERFORMS_ROLE* — test-plan author
- knowledge_gathering (KGA) — *SHARES* — identical skeleton
- test_plan_definition (TPD) — *SHARES* — identical skeleton
- knowledge_gathering (KGA) — *DIVERGES_IN* — domain engines
- test_plan_definition (TPD) — *DIVERGES_IN* — domain engines
- identical skeleton — *INCLUDES_COMPONENT* — `__init__`
- identical skeleton — *INCLUDES_COMPONENT* — `constants`
- identical skeleton — *INCLUDES_COMPONENT* — `monitoring`
- identical skeleton — *INCLUDES_COMPONENT* — `server`
- identical skeleton — *INCLUDES_COMPONENT* — `bridge`
- identical skeleton — *INCLUDES_COMPONENT* — `executor`
- identical skeleton — *INCLUDES_COMPONENT* — `models`
- identical skeleton — *DEFINES* — framework/contract layer
- knowledge_gathering (KGA) — *USES_DOMAIN_MODULE* — `loop/`
- `loop/` — *IS_A* — crawl engine
- knowledge_gathering (KGA) — *HAS_EXECUTOR_HANDLER* — `gather`
- knowledge_gathering (KGA) — *HAS_EXECUTOR_HANDLER* — `refine`
- test_plan_definition (TPD) — *USES_DOMAIN_MODULE* — `define/`
- test_plan_definition (TPD) — *USES_DOMAIN_MODULE* — `implement/`
- test_plan_definition (TPD) — *USES_DOMAIN_MODULE* — `llm/` (TPD local)
- test_plan_definition (TPD) — *USES_DOMAIN_MODULE* — `memory/`
- test_plan_definition (TPD) — *USES_DOMAIN_MODULE* — `render/`
- `render/` — *GENERATES* — gherkin
- test_plan_definition (TPD) — *HAS_EXECUTOR_HANDLER* — `define`
- test_plan_definition (TPD) — *HAS_EXECUTOR_HANDLER* — `implement`
- knowledge_gathering (KGA) — *LACKS_LOCAL_MODULE* — `llm/`
- knowledge_gathering (KGA) — *USES_LLM_FROM* — `common/llm`
- test_plan_definition (TPD) — *HAS_LOCAL_MODULE* — `llm/` (TPD local)
- test_plan_definition (TPD) — *USES* — domain-specific prompts
- `common` — *DOES_NOT_IMPORT* — A2A agents
- knowledge_gathering (KGA) — *DOES_NOT_IMPORT* — test_plan_definition (TPD)
- test_plan_definition (TPD) — *DOES_NOT_IMPORT* — knowledge_gathering (KGA)
- test-agent monorepo — *DEMONSTRATES_PRINCIPLE* — Service symmetry belongs at the contract layer not the domain layer
- Service symmetry belongs at the contract layer not the domain layer — *IS_RELATED_TO* — test-agent monorepo

%% ai-graph-end %%