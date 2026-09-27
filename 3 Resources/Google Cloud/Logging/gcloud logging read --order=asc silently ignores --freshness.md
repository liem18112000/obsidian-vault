---
ai_hash: 4d93a8bd501144d4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: PROD investigation 2026-08-03
status: seedling
tags:
- gcloud
- cloud-logging
- gotcha
- observability
title: 'gcloud logging read: --order=asc silently ignores --freshness'
type: gotcha
---

# gcloud logging read: --order=asc silently ignores --freshness

`gcloud logging read` applies `--freshness=<dur>` (the implicit `timestamp >= now-dur` bound) **only with the default `--order=desc`**. If you pass `--order=asc`, freshness is effectively ignored and the query returns the **oldest** entries in the whole retention window instead of recent ones.

## How it bit me
Counting "EnricherException in the last 12h" with `--order=asc --freshness=12h --limit=3000` returned 3000 rows all dated a **month earlier** (start of retention), not today. Switching to an explicit bound fixed it.

## Rule
For any time-bounded count/slice, pass an **explicit** `timestamp>="...Z"` (and `timestamp<=` if needed) in the filter rather than relying on `--freshness`, especially when you also want ascending order. `--freshness` is a convenience for the common `desc` "show me recent" case only.

## Related
[[Naive textPayload substring matching produces false-positive log hits]]

## Related

- [[Naive textPayload substring matching produces false-positive log hits]]

%% ai-graph-start %%

**Related notes:**
- [[gcloud logging read --freshness is unreliable with broad OR filters; use explicit timestamp bound]]
- [[GKE log FRESHNESS window gets swamped by noise on high-traffic clusters]]
- [[Naive textPayload substring matching produces false-positive log hits]]
- [[gcloud-logging-shard-field-vs-sharding-keyword]]
- [[Resolve Cloud Logging share links via redirect Location header]]

%% ai-graph-end %%