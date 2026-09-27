---
title: "Architecture"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457108704/Architecture
space: "AI"
topic: architecture
relevance: 0.746
depth: 2.44
updated: 2017-07-21
attachments: 5
tags:
  - confluence
  - architecture
  - space/ai
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
