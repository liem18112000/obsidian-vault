---
ai_hash: 471c5c9cd8d9391e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 2.44
entities: []
relevance: 0.746
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457108704/Architecture
space: AI
status: reference
tags:
- confluence
- architecture
- space/ai
title: Architecture
topic: architecture
type: source
updated: 2017-07-21
---

# Architecture

> [!info] Imported from Confluence
> Space **AI** · updated 2017-07-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457108704/Architecture)
> Relevance 0.746 · topic `architecture`

## Phase 1 – Collect and process as needed

Start collecting application data in a scalable data store. Analyse the raw data as needed.

Use technologies from a [standardised Apache Hadoop Ecosystem](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457108712/Vendors) (preferably HDP). The second slide exemplifies potential technologies.


![[2457108704-2017-07-05_TeamAI-Roadmap_mr-a.png]]

![[2457108704-2017-07-05_TeamAI-Roadmap-mr-b.png]]



Most important are the gateways to interact with the platform. They're used to push data and to query results.

We should also consider (evaluate) the following tools: Apache Impala, Presto and Apache Drill for enhanced analytics (and improved performance).

## Phase 2 – Process data more quickly

All data can be seen as a stream of events. Focus on transactions, not state.

Technologies in these area grow and die fast these days. The second slide exemplifies potential technologies.


![[2457108704-2017-07-05_TeamAI-Roadmap_stream-a.png]]

![[2457108704-2017-07-05_TeamAI-Roadmap_stream-b.png]]

%% ai-graph-start %%

**Related notes:**
- [[AI-Powered Development Environment Architecture]]
- [[Lamda Architecture]]
- [[Kappa Architecture]]
- [[IR - System Design]]

%% ai-graph-end %%