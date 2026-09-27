---
title: "Lamda Architecture"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457109074/Lamda+Architecture
space: "AI"
topic: architecture
relevance: 0.755
depth: 2.38
updated: 2017-07-14
attachments: 1
tags:
  - confluence
  - architecture
  - space/ai
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
