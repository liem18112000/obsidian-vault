---
ai_hash: 3e8493d425ea9e45
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Part A - luz-jsonstore Analysis (2025-12-18)'
status: seedling
tags:
- luz-jsonstore
- reliability
- root-cause-analysis
- kubernetes
- production
- kepler
title: Every production FAILED_TO_STORE traced back to a rolling deploy, not to load
type: observation
---

# Every production FAILED_TO_STORE traced back to a rolling deploy, not to load

A 30-day production review of `klara-prod` found **25 `FAILED_TO_STORE` errors in `luz-jsonstore`, and every single one lined up with a pod restart** — a rolling deployment in progress. Not load, not MongoDB, not payload size. 100% correlation, 11 distinct dates.

The technique that produced that answer is the reusable part: **put the error timestamps and the pod lifecycle timestamps on one timeline and look at the seconds column.** Case 27748 is the template —

| 14:15:50.468 | `connection refused` luz-docs-batch → luz-jsonstore |
| 14:15:50.839 | `FAILED_TO_STORE` documentId 27748 |
| 14:15:54.871 | pod `luz-jsonstore-…-7nnt8` starting |
| 14:15:55–14:16:23 | five more pods starting |

The error lands **~4 seconds before** the new pods appear — it is the tail of the *old* pods going away, which immediately rules out "new pod is slow to warm up" and points at shutdown and endpoint withdrawal instead.

Two supporting signals that sharpened it further: the failures clustered at **11:00 UTC** (the deploy window), and **23 of 25 were the same operation**, `updateOrRemoveMetadataFilterFields` — a single un-retried call path, not a general storage problem.

Lesson: **when errors are rare, bursty, and cluster on particular dates, correlate against deployments before investigating the application at all.** Rare-and-bursty is a deployment signature; steady-and-proportional-to-traffic is a capacity signature.

Root causes found: [[An exec-cat readiness probe reports Ready before the server can serve]] and [[A rolling deploy drops in-flight requests unless preStop outlives endpoint propagation]].

## Related

- [[An exec-cat readiness probe reports Ready before the server can serve]]
- [[A rolling deploy drops in-flight requests unless preStop outlives endpoint propagation]]
- [[Error volume and error severity are independent, so triage by impact not by count]]

## Related

- [[An exec-cat readiness probe reports Ready before the server can serve]]
- [[A rolling deploy drops in-flight requests unless preStop outlives endpoint propagation]]

%% ai-graph-start %%

**Related notes:**
- [[Part A - luz-jsonstore Analysis]]
- [[An exec-cat readiness probe reports Ready before the server can serve]]
- [[Case Report - DocumentId 778]]
- [[Case Report - DocumentId 588]]
- [[Case Report - DocumentId 21579]]

%% ai-graph-end %%