---
ai_hash: d0ed6e8baf05845e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Coding-agent prompt
- Codebase anchors
- House style
- Coding agent
- Mergeable code
- Plausible-looking rewrite
- Prompt
- Frontier models
- Real ticket
- Scope fence
- Backend API
- LUZ-147139
- luz_adyen service
- UI work
- Business problem
- Numbered facts
- Feature request
- PO
- Postman
- DB mapping
- Fee percentages
- New stores
- Default 1.7%
- Negotiated fee
- Exact paths
- Descriptions
- Resource
- Service
- Repository
- Client
- Tests
- Sample
- AdyenStoreEntity
- splitConfigurationId
- updateStoreSplitConfiguration(...)
- createStoreSplitConfiguration(...)
- defaultSplitConfiguration
- Architectural constraints
- Prohibitions
- Wrapper pattern
- DTO/mapper layer
- Caching
- Service level
- Resource level
- Client level
- Quarkus
- '@CacheResult'
- TTL 1 hour
- Ambiguous logic
- Validation rules
- Response contract
- Resolution order
- Fallback chain
- Competent engineer
- Codebase
- Test of a good agent prompt
- Benchmark harness
- Model's assumptions
- Manufacture structured disagreement when real model independence is unavailable
- Compare AI models gpt-5.4(medium) vs opus4.6(high)
- Helios
- Confluence
source: 'Confluence: Compare AI models gpt-5.4 vs opus4.6 (Helios)'
status: seedling
tags:
- llm
- coding-agent
- prompting
- claude-code
- code-generation
- confluence-distilled
title: A coding-agent prompt needs codebase anchors and stated house style
type: lesson
---

# A coding-agent prompt needs codebase anchors and stated house style

The difference between a coding agent that produces mergeable code and one that produces a plausible-looking rewrite is almost entirely in the prompt. A prompt used to benchmark two frontier models on the same real ticket shows the ingredients, and every one of them is doing work.

**1. A scope fence, stated first.**
> "Implement backend API only for LUZ-147139 in the `luz_adyen` service. **Do not do any UI work yet.**"

Without this an agent helpfully expands scope and you review three times the diff you asked for.

**2. The business problem as numbered facts, not a feature request.**
> PO changes split configuration manually in Postman · our DB mapping is not updated, so fee percentages display incorrectly · new stores fall back to the default 1.7% instead of the negotiated fee.

This is *why*, and it lets the agent make sensible calls on the parts you forgot to specify.

**3. Codebase anchors — exact paths, not descriptions.**
```
Resource:   src/main/java/ch/klara/luz/adyen/resource/StoreManagementResource.java
Service:    src/main/java/ch/klara/luz/adyen/service/AdyenStoreService.java
Repository: …/repository/AdyenStoreRepository.java
Client:     …/rest/adyen/client/SplitConfigurationMerchantLevelApiClient.java
Tests:      src/test/java/…/AdyenStoreServiceTest.java
Sample:     split_response.json
```
Naming the files that already exist is the single highest-leverage line in the prompt. It converts "write this feature" into "extend these seven files", which is a far smaller and more reviewable task.

**4. "Important existing behavior" — what the code does *today*.**
> `AdyenStoreEntity` already has `splitConfigurationId` · `updateStoreSplitConfiguration(...)` currently only refreshes local DB state · `createStoreSplitConfiguration(...)` falls back to `defaultSplitConfiguration`.

This is the anti-reinvention section. Absent it, the agent rebuilds what is already there under a new name.

**5. Architectural constraints, stated as prohibitions.**
> "Use the **wrapper pattern, not DTO/mapper layer**" · "Place caching at **service level, not resource/client level**" · "Use Quarkus `@CacheResult`, TTL 1 hour".

House style is invisible to a model reading a few files. Saying which of two reasonable patterns you use — and explicitly which you do *not* — prevents the most common review round-trip.

**6. The ambiguous logic spelled out.** Validation rules enumerated, the response contract listed field by field, and a **resolution order** for the fallback chain. Anywhere behaviour could go two ways, the prompt picks one.

> [!tip] The test of a good agent prompt
> Could a competent engineer who has never seen this codebase implement it from the prompt alone? If not, the agent is guessing at the same gaps — and unlike the engineer, it will not ask.

> [!note] Why this doubles as a benchmark harness
> Because the prompt is this specific, running two models against it compares *them* rather than comparing their willingness to guess. A vague prompt mostly measures which model's assumptions happen to match yours.

Related: [[Manufacture structured disagreement when real model independence is unavailable]].

Source: [[Compare AI models gpt-5.4(medium) vs opus4.6(high)]] (Helios, Confluence).

## Related

- [[Manufacture structured disagreement when real model independence is unavailable]]

%% ai-graph-start %%

**Related notes:**
- [[Compare AI models gpt-5.4(medium) vs opus4.6(high)]]
- [[Route tool-less LLM passes to a cheaper model tier]]
- [[Prompt Architecture Code Review]]
- [[Proposal An LLM Council for ePost – multi-lens deliberation in Claude Code]]
- [[Multi-Agentic Architecture - Apply in AI Driven Testing]]

**Relations:**
- Coding-agent prompt — *needs* — Codebase anchors
- Coding-agent prompt — *needs* — House style
- Coding agent — *produces* — Mergeable code
- Coding agent — *produces* — Plausible-looking rewrite
- Prompt — *influences* — Coding agent
- Prompt — *benchmarks* — Frontier models
- Prompt — *addresses* — Real ticket
- Scope fence — *is part of* — Prompt
- Scope fence — *prevents* — UI work
- Backend API — *for* — LUZ-147139
- LUZ-147139 — *in* — luz_adyen service
- Business problem — *as* — Numbered facts
- Business problem — *not* — Feature request
- PO — *uses* — Postman
- DB mapping — *is not updated* — Fee percentages
- Fee percentages — *display* — incorrectly
- New stores — *fall back to* — Default 1.7%
- Default 1.7% — *instead of* — Negotiated fee
- Codebase anchors — *are* — Exact paths
- Codebase anchors — *not* — Descriptions
- Codebase anchors — *include* — Resource
- Codebase anchors — *include* — Service
- Codebase anchors — *include* — Repository
- Codebase anchors — *include* — Client
- Codebase anchors — *include* — Tests
- Codebase anchors — *include* — Sample
- AdyenStoreEntity — *has* — splitConfigurationId
- updateStoreSplitConfiguration(...) — *refreshes* — local DB state
- createStoreSplitConfiguration(...) — *falls back to* — defaultSplitConfiguration
- Architectural constraints — *are* — Prohibitions
- Architectural constraints — *specify* — Wrapper pattern
- Architectural constraints — *exclude* — DTO/mapper layer
- Architectural constraints — *specify* — Caching
- Caching — *at* — Service level
- Caching — *not at* — Resource level
- Caching — *not at* — Client level
- Caching — *uses* — Quarkus
- Quarkus — *provides* — @CacheResult
- @CacheResult — *has* — TTL 1 hour
- Ambiguous logic — *includes* — Validation rules
- Ambiguous logic — *includes* — Response contract
- Ambiguous logic — *includes* — Resolution order
- Resolution order — *for* — Fallback chain
- Prompt — *picks* — one
- Competent engineer — *implements from* — Prompt
- Test of a good agent prompt — *is* — Competent engineer
- Benchmark harness — *compares* — Frontier models
- Vague prompt — *measures* — Model's assumptions
- Coding-agent prompt — *is related to* — Manufacture structured disagreement when real model independence is unavailable
- Coding-agent prompt — *is related to* — Compare AI models gpt-5.4(medium) vs opus4.6(high)
- Compare AI models gpt-5.4(medium) vs opus4.6(high) — *source* — Helios
- Compare AI models gpt-5.4(medium) vs opus4.6(high) — *source* — Confluence

%% ai-graph-end %%