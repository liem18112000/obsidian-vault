---
ai_hash: 69ca4362ca95ea0c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530456200/Script+to+create+Redirection+Order
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Script to create Redirection Order
topic: programming
type: source
updated: 2021-07-08
---

# Script to create Redirection Order

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-07-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530456200/Script+to+create+Redirection+Order)
> Relevance 0.731 · topic `programming`

### Problem

We have a problem and cannot send the Redirection Order mail for tenant(*d575285f-cdf6-47c4-8649-f7982ab129a5*) who already subscribed the Scanning as business widget: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20530456200_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-59186" macro-id="fec09268-9698-4942-bdd8-3e8691ad9662" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-59186" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-59186</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

### Fix

Script to create redirection order for tenant: *d575285f-cdf6-47c4-8649-f7982ab129a5*

All codes are put in this folder: <a href="https://bitbucket.org/axonivy-prod/luz_devops/src/master/klara-maintenance-scripts/src/main/k8s/redirection-order/" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_devops/src/master/klara-maintenance-scripts/src/main/k8s/redirection-order/</a>

Step to run

1.Run this script

*<a href="https://bitbucket.org/axonivy-prod/luz_devops/src/master/klara-maintenance-scripts/src/main/k8s/redirection-order/run-create-redirection-order.sh" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_devops/src/master/klara-maintenance-scripts/src/main/k8s/redirection-order/run-create-redirection-order.sh</a>*

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="37901956-c98d-43c0-a7f5-560bbc276e9c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
sh run-create-redirection-order.sh d575285f-cdf6-47c4-8649-f7982ab129a5
```

</div>

</div>

2\. Check the log by this script

<a href="https://bitbucket.org/axonivy-prod/luz_devops/src/master/klara-maintenance-scripts/src/main/k8s/redirection-order/logs-create-redirection-order.sh" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_devops/src/master/klara-maintenance-scripts/src/main/k8s/redirection-order/logs-create-redirection-order.sh</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c14df108-b85c-4bc9-a4ca-7806228e4989" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
sh logs-create-redirection-order.sh
```

</div>

</div>

The result looks like this

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="979d9755-31c3-4ecc-8042-b6c8e4dbcc10" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start create redirection order for tenant: {TENANT-ID}
Result: HTTP_STATUS: 200
Finished
```

</div>

</div>

3\. Clean up

<a href="https://bitbucket.org/axonivy-prod/luz_devops/src/master/klara-maintenance-scripts/src/main/k8s/redirection-order/cleanup-create-redirection-order.sh" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_devops/src/master/klara-maintenance-scripts/src/main/k8s/redirection-order/cleanup-create-redirection-order.sh</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2a6ab774-383e-41c1-a839-b154713707d8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
sh cleanup-create-redirection-order.sh
```

</div>

</div>

Note: We need to set current context for kubectl that is Prod.

%% ai-graph-start %%

**Related notes:**
- [[Rerun Own domain migration api for all tenant]]
- [[Run Script Resync hidden wiget]]
- [[14. Create companies by tenant id]]
- [[15. Update companies by tenant id]]
- [[How to execute API to create sync event for post from tenant schemas to public table]]

%% ai-graph-end %%