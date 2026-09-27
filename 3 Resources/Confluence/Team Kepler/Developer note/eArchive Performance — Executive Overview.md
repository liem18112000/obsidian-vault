---
ai_hash: 2fd91af8cb2acb5d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49505108142'
confluence_path: Team Kepler > Developer note
created: 2026-06-15
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- earchive
- performance
title: eArchive Performance — Executive Overview
type: source
updated: 2026-06-15
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49505108142/eArchive+Performance+Executive+Overview
---

# eArchive Performance — Executive Overview

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49505108142/eArchive+Performance+Executive+Overview) · updated 2026-06-15*

## Problem

eArchive is critically slow for large tenants. A production tenant with **128K+ documents** takes **~45s to open** and **~3 min to find a document** — caused by inefficient MongoDB queries scanning the entire collection on every request.

## What We've Done ✅

- **Backend query optimization** — removed expensive folder lookups and full collection scans from the hot path; the largest tenant workload is **128K+ documents**, but the UI only needs **48 documents** per page.

- **Latency reduction delivered** (simulation test on Dev + Performance)— addressed the biggest backend bottlenecks with estimated savings of **20–30s** from search/count optimization, **5–10s** from indexing, and **3–5s** from simplified query execution.

- **Develop data migration campaign completed** — materialized folder and permission fields for existing tenants so search/count/facet/get by document ID APIs can avoid repeated cross-collection joins.

- **Scalability foundation in place** — initial MongoDB indexes added and materialized query path enabled; target remains **under 10s** for the full eArchive workflow.

## Target

**Under 10 seconds** for the full eArchive workflow, even at 128K+ documents.

- [Performance Analysis and Proposed Solutions](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49317347331/Performance+Analysis+and+Proposed+Solutions)

- [eArchive performance — luz-epost-business-web calls the count API on every search](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49492099078/eArchive+performance+luz-epost-business-web+calls+the+count+API+on+every+search)

## References

For more details progress view, please visit: [https://axonivy.atlassian.net/wiki/x/xQC1hgs](https://axonivy.atlassian.net/wiki/x/xQC1hgs)

## Attachments

*Attached to the Confluence page but not embedded in its body.*

- [[3 Resources/Confluence/Team Kepler/Developer note/attachments/earchive-performance-executive-overview/story.svg|story.svg]]

%% ai-graph-start %%

**Related notes:**
- [[eArchive Performance — Detail Overview]]
- [[Performance Analysis and Proposed Solutions]]
- [[eArchive Performance measurement & scalability assessment at 2.2M documents]]
- [[eArchive 800k bottleneck is view-controller not K]]
- [[Reproduce performance issue and understand the issue on DEV - A.Vu]]

%% ai-graph-end %%