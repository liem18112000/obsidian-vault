---
ai_hash: bec25c63b231c9b5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-31
entities: []
source: session 2026-08-24 ecomart-java architecture analysis
status: seedling
tags:
- spring
- dependency-injection
- beans
- gotcha
- architecture
title: Spring injects List/Map beans by generic type, which is fragile
type: lesson
---

# Spring injects List/Map beans by generic type, which is fragile

When a Spring `@Configuration` exposes container beans like `List<PricePlan>` or `Map<String,String>` and a component asks for that exact generic type in its constructor, Spring resolves the injection **by the generic type signature**. This works but is brittle:

- Two `Map<String, ?>` beans distinguished **only by their value type** (`Map<String,String>` vs `Map<String,List<ElectricityReading>>`) rely on generic-type matching to disambiguate. Add a third map with a colliding signature and injection becomes ambiguous or wrong.
- Injecting `List<PricePlan>` can also collide with Spring's *collection injection* semantics (it may try to gather all `PricePlan` beans into a list rather than inject your one list bean).

**Safer patterns:** wrap collections in a named domain type (e.g. `PricePlanCatalog`, `MeterReadingStore`) and inject that; or use `@Qualifier` with explicit bean names; or move the data behind a repository interface. Domain wrapper types also give you a place for behavior and stop primitive/collection obsession.

Symptom to watch for: DI "works by luck" because no two beans happen to share a generic signature yet.

%% ai-graph-start %%

**Related notes:**
- [[New collaborator call NPEs old @InjectMocks tests]]

%% ai-graph-end %%