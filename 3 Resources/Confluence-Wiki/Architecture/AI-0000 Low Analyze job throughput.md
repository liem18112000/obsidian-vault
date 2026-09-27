---
title: "AI-0000 Low Analyze job throughput"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/48407216129/AI-0000+Low+Analyze+job+throughput
space: "AI"
topic: architecture
relevance: 0.711
depth: 2.17
updated: 2025-03-19
attachments: 0
tags:
  - confluence
  - architecture
  - space/ai
---

# AI-0000 Low Analyze job throughput

> [!info] Imported from Confluence
> Space **AI** · updated 2025-03-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/48407216129/AI-0000+Low+Analyze+job+throughput)
> Relevance 0.711 · topic `architecture`

<div class="plugin-tabmeta-details conf-macro output-block" hasbody="true" macro-id="07eb05d7-9990-4a94-bb8c-40a476067374" macro-name="details">

<div>

|                |                                 |
|----------------|---------------------------------|
| **Jira Issue** | *\<Link to issue and/or epic\>* |

</div>

</div>

# Table of content

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of content" hasbody="false" headerelements="H1,H2" macro-id="0ee09a57-a0fb-419e-bc91-b25e276871ee" macro-name="toc">

</div>

# Introduction

Observations we make on PROD

- Throughput of Analyze jobs seems to be low.

- We see many transaction log timeouts

- We see a lot of scale-out/down events

Evaluation setting

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="eb4ffbad-6e45-47ad-8888-aa5539692e52" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl run analyze-loadrun --namespace analyze --rm=true -i \
        --image=europe-west6-docker.pkg.dev/klara-ai-dev-01/d/kai-docker-loadrun:latest \
        -- LoadRun.sh --owner=tho --endpointUrl=http://analyze/api/v2 \
        --minBatchSize=1000 --queueSize=1000 --limit=1000 \
        --scene=LetterRcptTenantIdDevScene \
        --priority=DEFAULT --id=tho_2025-03-14_01
```

</div>

</div>

# Steps

## Step 1 - *Single node cluster without autoscaling*

<div>

|  |  |
|----|----|
| **ID** | `tho_2025-03-14_01` |
| **Clients** | 1 |
| **Total** | 1000 |
| **Failed** | 0 |
| **StartedAt** | 2025-03-14T16:16:25.177307978Z |
| **CompletedAt** | 2025-03-14T16:32:51.742790879Z |
| **Total Duration** | 16m 26.565482901s |
| **Avg Time/Job** | 986 |
| **API Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/a7cb1440-8d76-42e7-afd8-b6afa6184fec;startTime=2025-03-14T16:16:00.000Z;endTime=2025-03-14T16:37:00.000Z;filters=type:rlabel,key:namespace_name,val:analyze%2Btype:mlabel,key:owner,val:tho?inv=1&amp;invt=AbsBcA&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22,%22VM_TERMINATION%22%5D))" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/API-metrics</a> |
| **Job Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/b67aab73-4135-4af7-9e2a-406c155b6c54;startTime=2025-03-14T16:16:00.000Z;endTime=2025-03-14T16:37:00.000Z?inv=1&amp;invt=AbsBcA&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22%5D))" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/Job-metrics</a> |

</div>

## Step 2 - *Two node cluster without autoscaling*

<div>

|  |  |
|----|----|
| **ID** | `tho_2025-03-17_01` |
| **Clients** | 1 |
| **Total** | 1000 |
| **Failed** | 0 |
| **StartedAt** | 2025-03-17T09:11:30.602103175Z |
| **CompletedAt** | 2025-03-17T09:33:02.355512625Z |
| **Total Duration** | 21m 31.75340945s (+5min 5s) |
| **Avg Time/Job** | 1291 |
| **API Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/a7cb1440-8d76-42e7-afd8-b6afa6184fec;filters=type:rlabel,key:namespace_name,val:analyze%2Btype:mlabel,key:owner,val:tho;startTime=2025-03-17T09:10:00.000Z;endTime=2025-03-17T09:37:00.000Z?inv=1&amp;invt=AbsQlw&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22,%22VM_TERMINATION%22%5D))" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/API-metrics</a> |
| **Job Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/b67aab73-4135-4af7-9e2a-406c155b6c54;startTime=2025-03-17T09:10:00.000Z;endTime=2025-03-17T09:37:00.000Z?inv=1&amp;invt=AbsQlw&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22%5D))" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/Job-metrics</a> |

</div>

## Step 3 - *Three node cluster without autoscaling*

<div>

|  |  |
|----|----|
| **ID** | `tho_2025-03-17_03` |
| **Clients** | 1 |
| **Total** | 1000 |
| **Failed** | 0 |
| **StartedAt** | 2025-03-17T12:49:37.770034977Z |
| **CompletedAt** | 2025-03-17T13:08:58.180960277Z |
| **Total Duration** | 19m 20.4109253s |
| **Avg Time/Job** | 1160 |
| **API Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/a7cb1440-8d76-42e7-afd8-b6afa6184fec;filters=type:rlabel,key:namespace_name,val:analyze%2Btype:mlabel,key:owner,val:tho;startTime=2025-03-17T12:48:00.000Z;endTime=2025-03-17T13:12:00.000Z?inv=1&amp;invt=AbsRfg&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22,%22VM_TERMINATION%22%5D))" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/API-metrics</a> |
| **Job Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/b67aab73-4135-4af7-9e2a-406c155b6c54;startTime=2025-03-17T12:48:00.000Z;endTime=2025-03-17T13:12:00.000Z?inv=1&amp;invt=AbsRfg&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22%5D))&amp;pli=1" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/Job-metrics</a> |

</div>

## Step 3 - *Three node cluster without autoscaling*

<div>

|  |  |
|----|----|
| **ID** | `tho_2025-03-17_03` |
| **Clients** | 3 |
| **Total** | 999 |
| **Failed** | 0 |
| **StartedAt** | 2025-03-17T13:19:01.787284013Z |
| **CompletedAt** | 2025-03-17T13:29:49.899337333Z |
| **Total Duration** | 10m 48.11205332s |
| **Avg Time/Job** | 648 |
| **API Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/a7cb1440-8d76-42e7-afd8-b6afa6184fec;filters=type:rlabel,key:namespace_name,val:analyze%2Btype:mlabel,key:owner,val:tho;startTime=2025-03-17T13:18:00.000Z;endTime=2025-03-17T13:34:00.000Z?inv=1&amp;invt=AbsRoQ&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22,%22VM_TERMINATION%22%5D))" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/API-metrics</a> |
| **Job Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/b67aab73-4135-4af7-9e2a-406c155b6c54;startTime=2025-03-17T13:18:00.000Z;endTime=2025-03-17T13:34:00.000Z?inv=1&amp;invt=AbsRog&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22%5D))&amp;pli=1" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/Job-metrics</a> |

</div>

## Step 3 - *Three node cluster without autoscaling*

<div>

|  |  |
|----|----|
| **ID** | `tho_2025-03-18_2` |
| **Clients** | 5 |
| **Total** | 1000 (5 x 200) |
| **Failed** | 0 |
| **StartedAt** | 2025-03-18T16:41:09.039144142Z |
| **CompletedAt** | 2025-03-18T16:55:09.335703371Z |
| **Total Duration** | 14m 0.296S |
| **Avg Time/Job** | 840 |
| **API Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/a7cb1440-8d76-42e7-afd8-b6afa6184fec;startTime=2025-03-18T16:40:00.000Z;endTime=2025-03-18T16:58:00.000Z;filters=type:rlabel,key:namespace_name,val:analyze?inv=1&amp;invt=AbsYBQ&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22,%22VM_TERMINATION%22%5D))" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/API-metrics</a> |
| **Job Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/b67aab73-4135-4af7-9e2a-406c155b6c54;startTime=2025-03-18T16:40:00.000Z;endTime=2025-03-18T16:58:00.000Z?inv=1&amp;invt=AbsYBQ&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22%5D))" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/Job-metrics</a> |

</div>

## Step 3 - *Three node cluster without autoscaling*

<div>

|  |  |
|----|----|
| **ID** | `tho_2025-03-19_03` |
| **Clients** | 5 |
| **Total** | 2000 (5 x 400) |
| **Failed** | 0 |
| **StartedAt** | 2025-03-19T07:39:50.758880795Z |
| **CompletedAt** | 2025-03-19T08:05:47.837256293Z |
| **Total Duration** | 25m 57.078375498s |
| **Avg Time/Job** | 1560 |
| **API Metrics** | <a href="https://console.cloud.google.com/monitoring/dashboards/builder/a7cb1440-8d76-42e7-afd8-b6afa6184fec;filters=type:rlabel,key:namespace_name,val:analyze;startTime=2025-03-19T07:38:00.000Z;endTime=2025-03-19T08:09:00.000Z?inv=1&amp;invt=AbsbWA&amp;project=klara-ai-dev-01&amp;pageState=(%22eventTypes%22:(%22selected%22:%5B%22CLOUD_ALERTING_ALERT%22,%22GKE_WORKLOAD_DEPLOYMENT%22,%22VM_TERMINATION%22%5D))" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboard/API-metrics</a> |
| **Job Metrics** | no data… |

</div>

## Step X

### How

*How can this step be reproduced*

### Result / Suggested next steps

*What was the result? Was it as expected? What are the potential next steps?*  

# Conclusion

*What did we learn? Was the issue solved? And if not why did we stop the attempt to solve it?*
