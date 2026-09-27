---
title: "One Api Retry Mechanism"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48527540579/One+Api+Retry+Mechanism
space: "LUZ"
topic: programming
relevance: 0.769
depth: 2.68
updated: 2025-06-05
attachments: 2
tags:
  - confluence
  - programming
  - space/luz
---

# One Api Retry Mechanism

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-06-05 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48527540579/One+Api+Retry+Mechanism)
> Relevance 0.769 · topic `programming`

Reference Story: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48527540579_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-135464" macro-id="1bef3a04-dd57-4f9d-a422-3449b1273093" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-135464" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-135464</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>


![[48527540579-OneAPI retry mechanism.png]]



Retry mechanism that is being used by one api

1.  Faultolerance

    1.  Retry with delay time

    2.  Fallback and alert

2.  Retry with PubSubRetry

    1.  \(1\) If fails to publish retry msg to queue then update status to fail

    2.  \(2\) If fails to publish retry msg to queue then nack msg

3.  CronJob Unlimited Retry

    1.  retry-failed-to-store cron job

    2.  delivery physical document cron job
