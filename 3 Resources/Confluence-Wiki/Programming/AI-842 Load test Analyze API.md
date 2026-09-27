---
title: "AI-842 Load test Analyze API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2530763997/AI-842+Load+test+Analyze+API
space: "AI"
topic: programming
relevance: 0.736
depth: 2.41
updated: 2021-04-12
attachments: 4
tags:
  - confluence
  - programming
  - space/ai
---

# AI-842 Load test Analyze API

> [!info] Imported from Confluence
> Space **AI** · updated 2021-04-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2530763997/AI-842+Load+test+Analyze+API)
> Relevance 0.736 · topic `programming`

*<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_2530763997_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="AI-842" macro-id="b1a10ed8-23b9-4839-919d-fba9956d7007" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/AI-842" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>AI-842</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>  *

# Table of content

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of content" hasbody="false" headerelements="H1,H2" macro-id="4658f83c-a12a-4d82-bb2d-829398dc8547" macro-name="toc">

</div>

# Introduction

Measure throughput of Analyze API cluster with different scenarios.

# Steps

## Step 1 - Document.Discover task with scanned PDF as input

### What

Measure time to process 100, 200 and 300 jobs on a two node Analyze API cluster.

### How

Use Apache JMeter with configuration <a href="https://bitbucket.org/axonivy-prod/com.axonivy.ai.dev/src/master/usr/jmeter/analyze-api/AnalyzeApi-Heavy.jmx" class="external-link" rel="nofollow">AnalyzeApi-Heavy.jmx</a>.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="28055d92-e06f-4a88-9b8c-e261e3f43935" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
jmeter.sh -n -t ~/Source/com.axonivy.ai.dev/usr/jmeter/analyze-api/AnalyzeApi-Heavy.jmx -l ~/Downloads/jmeter-result.csv -e -o Downloads/jmeter-result
```

</div>

</div>

### Results

The following charts show the number of transactions (submit, wait for result, read job status, delete job) per second.

Every test starts with a ramp-up and ends with a run-down phase.

#### 100 jobs on two node cluster (2.4 seconds / job)


![[2530763997-100-2-flotTransactionsPerSecond.png]]



#### 200 jobs on two node cluster (1.85 seconds / job)

The green line represents job submission. You can see that more jobs are submitted that can be processed at the same time. Nevertheless, we have a higher throughput.


![[2530763997-200-2-flotTransactionsPerSecond.png]]



#### 300 jobs on two node cluster (1.93 seconds / job)

### 

![[2530763997-300-2-flotTransactionsPerSecond.png]]



#### 200 jobs on a single node (3.55 seconds / job)


![[2530763997-200-1-flotTransactionsPerSecond.png]]



#### 1 job on a single node (7 seconds / job)

This is the time used to submit, to wait for the results and to delete the job.

# Conclusion

The new asynchronous API allows us to process twice as many jobs in the same time.

Scaling out from one to two nodes nearly doubles the throughput again.
