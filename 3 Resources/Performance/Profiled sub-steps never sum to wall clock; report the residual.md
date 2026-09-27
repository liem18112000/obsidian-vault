---
ai_hash: 45fe38856534fa57
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: REST API calculate 1 employee pay-slip (LUZ)'
status: seedling
tags:
- profiling
- performance
- jvm
- hibernate
- measurement
- confluence-distilled
title: Profiled sub-steps never sum to wall clock; report the residual
type: lesson
---

# Profiled sub-steps never sum to wall clock; report the residual

When you profile an operation by timing its named sub-steps, the sub-steps **will not add up to the wall-clock total** — and the gap is not measurement error. It is real time spent in code you did not instrument.

A pay-slip calculation analysis states the caveat plainly:

> *"The estimated total time may not be equal to sum of all sub-steps. It is because we may have some calculations not listed in the below table, some extra time taken by the EJB container, CDI container, Hibernate framework, etc."*

**Where the residual lives:**

- **Container and framework overhead** — EJB/CDI interception, proxy creation, transaction begin/commit, dependency injection at call boundaries.
- **ORM machinery** — Hibernate session flushes, dirty checking, lazy-load proxies resolving at moments no line of your code names.
- **Serialisation at the edges** — JSON marshalling of the response is real time that belongs to no business sub-step.
- **Steps nobody listed** — the honest one. Instrumentation covers what someone thought to instrument.

**Why this matters for what you do next.** If sub-steps account for 60% of the time, optimising the slowest sub-step caps out at improving 60% of the problem. The residual is frequently the single largest line item, and it responds to entirely different fixes — fewer transaction boundaries, fewer proxied calls, a flush strategy — than the arithmetic inside any one step.

> [!tip] Always report the residual as a row
> Add an explicit **"unaccounted"** line to the table: `total − Σ(sub-steps)`. It costs nothing, it stops readers from silently assuming the parts are the whole, and a residual that grows between runs is itself a finding. A profile whose rows sum neatly to 100% is usually one that quietly dropped the remainder.

> [!warning] Local timings carry local overhead
> This measurement was taken on `localhost` with ~30 employees. Container overhead is roughly fixed per call while business logic scales with data, so the residual's *share* shrinks as data grows — meaning a local profile systematically overstates framework cost and understates the query problem. Re-measure at production data volume before concluding which half to attack.

Related: [[Split page load into server, render and interaction before optimising]] — decomposition is still the right first move; this note is about its limit.

Source: [[REST API calculate 1 employee's pay-slip]] (LUZ, Confluence).

## Related

- [[Split page load into server, render and interaction before optimising]]

%% ai-graph-start %%

**Related notes:**
- [[Split page load into server, render and interaction before optimising]]
- [[N+1 hides at the service-call layer too, not just in the ORM]]
- [[Analyze performance for REST API calculate payslips for overview salary processing]]
- [[Analyze N+1 queries for REST API calculate payslip for 1 employee]]
- [[REST API calculate 1 employee's pay-slip]]

%% ai-graph-end %%