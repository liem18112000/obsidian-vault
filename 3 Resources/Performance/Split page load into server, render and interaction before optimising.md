---
ai_hash: ac65f824c81541de
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: eLetter Performance Investigation Report (Helios)'
status: seedling
tags:
- performance
- frontend
- react
- ttfb
- profiling
- server-timing
- confluence-distilled
title: Split page load into server, render and interaction before optimising
type: lesson
---

# Split page load into server, render and interaction before optimising

"The page takes 5.6 seconds" is not actionable, because no single team owns it. Split the number into **server time, client render time, and interaction time** — the split usually shows the bottleneck is not where people assumed.

A real breakdown for a 48-item list view, against a **< 1,000 ms** target:

| Segment | Time | Owner |
|---|--:|---|
| **TTFB** (server-side work) | 1,432 ms | Backend / middleware |
| **React list render** | 2,625 ms | Frontend |
| **Click dispatch delay** (click → render begins) | 1,046 ms | Frontend interaction |
| Network transfer (RSC stream) | *relatively cheap* | — |
| **Total — list** | **5,620 ms** | ❌ 5.6× over |
| **Total — detail view** | **3,170 ms** | ❌ 3.2× over |

The finding that changes what you do: **client-side rendering cost more than the server** — 2,625 ms of React rendering against 1,432 ms of TTFB — and **network transfer was the cheapest part**. A team that assumed "the API is slow" would have optimised the smaller half of the problem.

**Why this decomposition is the right first move:**

- **It assigns ownership.** Each segment maps to a different team and a different class of fix. One aggregate number produces a meeting; three numbers produce three tickets.
- **It exposes the cheap wins.** Network being cheap means payload-shrinking work — usually the first instinct — would have bought almost nothing here.
- **It catches interaction cost, which aggregates hide.** 1,046 ms between click and render start is invisible in any page-load metric, yet it is the part the user experiences as "unresponsive". Perceived responsiveness and load time are different problems.

> [!tip] Measure against a stated threshold, not against "before"
> This report fixes **< 1,000 ms** up front and marks every scenario PASS/FAIL against it. That converts "is this fast enough?" from a judgement call into a check, and it stays meaningful after the code changes — whereas "30% faster than last month" tells you nothing about whether it is now acceptable.

> [!warning] Note what the numbers cannot tell you yet
> TTFB of 1,432 ms is *"server-side work that should be reduced by middleware optimization not yet deployed"* — i.e. one number covering your own SSR plus every upstream call. Until you emit `Server-Timing`, you cannot say which. Decompose as far as your instrumentation allows, and be explicit about where the resolution stops.

Related: [[N+1 hides at the service-call layer too, not just in the ORM]] — the same discipline applied to the backend half of a request.

Source: [[eLetter Performance - Investigation Report Load Times and Optimization Recommendations]] (Helios, Confluence).

## Related

- [[N+1 hides at the service-call layer too, not just in the ORM]]

%% ai-graph-start %%

**Related notes:**
- [[eLetter Performance - Investigation Report Load Times and Optimization Recommendations]]
- [[Profiled sub-steps never sum to wall clock; report the residual]]
- [[N+1 hides at the service-call layer too, not just in the ORM]]
- [[For authenticated pages pick the perf tool that can log in, not the prettiest report]]
- [[Measure component render timing with Playwright addInitScript]]

%% ai-graph-end %%