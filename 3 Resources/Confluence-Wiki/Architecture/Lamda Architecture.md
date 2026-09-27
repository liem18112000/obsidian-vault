---
ai_hash: bdefefc496d7e28b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.38
entities: []
relevance: 0.755
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457109074/Lamda+Architecture
space: AI
status: reference
tags:
- confluence
- architecture
- space/ai
title: Lamda Architecture
topic: architecture
type: source
updated: 2017-07-14
---

# Lamda Architecture

> [!info] Imported from Confluence
> Space **AI** · updated 2017-07-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457109074/Lamda+Architecture)
> Relevance 0.755 · topic `architecture`

Lambda architecture is a data-processing architecture designed to handle massive quantities of data by taking advantage of both batch- and stream-processing methods. This approach to architecture attempts to balance latency, throughput, and fault-tolerance by using batch processing to provide comprehensive and accurate views of batch data, while simultaneously using real-time stream processing to provide views of online data. The two view outputs may be joined before presentation.


![[2457109074-lambda.png]]



## Critism

The batch and streaming sides each require a different code base that must be maintained and kept in sync so that processed data produces the same result from both paths.

In a technical discussion over the merits of employing a pure streaming approach, it was noted that using a flexible streaming framework such as Apache Samza could provide some of the same benefits as batch processing without the latency.

## See also

<a href="http://lambda-architecture.net" class="external-link" rel="nofollow">http://lambda-architecture.net</a>

%% ai-graph-start %%

**Related notes:**
- [[Kappa Architecture]]
- [[Recipe Best practices implementing scalable distributed applications on cloud]]
- [[Architecture (2457108704)]]
- [[Synapse - ServerlessWorkflow Architecture overview]]

%% ai-graph-end %%