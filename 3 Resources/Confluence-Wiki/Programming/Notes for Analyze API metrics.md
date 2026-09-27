---
title: "Notes for Analyze API metrics"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/47121695531/Notes+for+Analyze+API+metrics
space: "AI"
topic: programming
relevance: 0.703
depth: 2.41
updated: 2022-06-02
attachments: 0
tags:
  - confluence
  - programming
  - space/ai
---

# Notes for Analyze API metrics

> [!info] Imported from Confluence
> Space **AI** · updated 2022-06-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/47121695531/Notes+for+Analyze+API+metrics)
> Relevance 0.703 · topic `programming`

Processor metrics.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8d8dc024-75c5-4e41-aafe-bc9d7d11ac23" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Processor / hour / priority / node|global {
inputCount                    --- number of distinct waiting elements
p80WaitingCount               --- p{80} number of elements waiting
maxWaitingCount               --- max number of elements waiting

p80WaitingTime                --- p{80} time an element waited
maxWaitingTime                --- max time an element waited

outputCount                   --- number of distinct completed elements
backPressure                  --- metric representing the pressure on the processor

p80ParallelProcessingCount    --- p{80} number of elements processed in parallel
maxParallelProcessingCount    --- maximum number of elements processed in parallel

idleTimePercentage            --- Precentage of time no elements where processed
totalProcessingTime           --- Sum of processing time of all elements within the current hour
}
```

</div>

</div>
