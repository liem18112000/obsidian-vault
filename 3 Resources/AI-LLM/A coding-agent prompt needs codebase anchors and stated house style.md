---
title: "A coding-agent prompt needs codebase anchors and stated house style"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Compare AI models gpt-5.4 vs opus4.6 (Helios)"
tags: [llm, coding-agent, prompting, claude-code, code-generation, confluence-distilled]
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
