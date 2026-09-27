---
title: "Measure API luz-docs"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47390916697/Measure+API+luz-docs
space: "LUZ"
topic: programming
relevance: 0.87
depth: 3
updated: 2023-06-07
attachments: 36
tags:
  - confluence
  - programming
  - space/luz
---

# Measure API luz-docs

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-06-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47390916697/Measure+API+luz-docs)
> Relevance 0.87 · topic `programming`

File using during testing (150KB)

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="c8a99079-12b6-4615-b8e3-3f06db417597" macro-name="view-file"><a href="../_attachments/47390916697-test.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/47390916697/test.pdf?version=1&amp;modificationDate=1685604167768&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[47390916697-test.pdf]]

</a></span>

## 1. ***Measure API store luz-docs.***

***POST /luz_docs/api/{tenantId}/documents***

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="0a6e7897-8bf2-4a00-98b8-6045363814b7" macro-name="view-file"><a href="../_attachments/47390916697-store document.html" class="confluence-embedded-file" data-nice-type="HTML Document" data-file-src="/wiki/download/attachments/47390916697/store%20document.html?version=1&amp;modificationDate=1685512626848&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/html" data-has-thumbnail="true">

![[47390916697-store document.html]]

</a></span>


![[47390916697-image-20230531-053305.png]]



It’s slow like which arrow team mention in <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47347040506/Load+test+luz+docs+and+related+modules?replyToComment=47376597368#comment-47376597368" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47347040506/Load+test+luz+docs+and+related+modules?replyToComment=47376597368#comment-47376597368</a>

**CPU**


![[47390916697-image-20230531-053428.png]]



**RAM**


![[47390916697-image-20230531-053440.png]]



Measure time metric with **DocumentCreatingService.createDocument**

Link <a href="https://console.cloud.google.com/monitoring/dashboards/builder/a6c890ec-d0e3-452c-b80c-59908a2a0441;startTime=2023-05-31T03:25:55.758Z;endTime=2023-05-31T04:00:07.158Z?project=klara-performance&amp;dashboardBuilderState=%257B%2522editModeEnabled%2522:true%257D" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboards/builder/a6c890ec-d0e3-452c-b80c-59908a2a0441;startTime=2023-05-31T03:25:55.758Z;endTime=2023-05-31T04:00:07.158Z?project=klara-performance&amp;dashboardBuilderState=%257B%2522editModeEnabled%2522:true%257D</a>


![[47390916697-image-20230531-053702.png]]



From the three charts, it can be observed that both RAM and CPU usage increased from the start until 10:28 AM. During that time, the execution time of the **createDocument** function also increased. I traced a request before 10:28 AM to see what happened within it.


![[47390916697-image-20230531-054826.png]]

![[47390916697-image-20230531-113409.png]]



There are time intervals marked in the image as 638ms and 3106ms where there are no incoming or outgoing requests. I assume that during these time intervals, the luz-doc server is performing computations (possibly on a file)

*Trace URL for image upon (without EnricherProcess)*: <a href="https://console.cloud.google.com/traces/list?tid=0104170e597edffae121e76dc27b8f49&amp;project=klara-performance&amp;pageState=(%22traceIntervalPicker%22:(%22groupValue%22:%22PT6H%22,%22customValue%22:null))" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=0104170e597edffae121e76dc27b8f49&amp;project=klara-performance&amp;pageState=("traceIntervalPicker":("groupValue":"PT6H","customValue":null))</a>

*Trace URL (with EnricherProcess)* : <a href="https://console.cloud.google.com/traces/list?tid=46cd66a6a7ed77505f997406e59edfab&amp;project=klara-performance" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=46cd66a6a7ed77505f997406e59edfab&amp;project=klara-performance</a>

*time of createUploadInputStreamMap and .getReferenceFileInputStream*

**More request:**

Trace link: <a href="https://console.cloud.google.com/traces/list?tid=09a0529a7069a854d70bda00c9e7949f" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=09a0529a7069a854d70bda00c9e7949f</a>

Time: 1111.263 ms

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="16489e7d-de40-4dfe-a0f4-48f933194700" macro-name="view-file"><a href="../_attachments/47390916697-Invalid file id - 6d4df1ae-7457-4df3-bcd4-d22ea70fb17c" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47390916697/Invalid%20file%20id%20-%206d4df1ae-7457-4df3-bcd4-d22ea70fb17c?version=1&amp;modificationDate=1685593423026&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[47390916697-Invalid file id - 6d4df1ae-7457-4df3-bcd4-d22ea70fb17c]]

</a></span>

Trace link: <a href="https://console.cloud.google.com/traces/list?tid=09b730b0ad6c00ab09394fb687644bc7" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=09b730b0ad6c00ab09394fb687644bc7</a>

Time: 2619.299 ms 


![[47390916697-image-20230601-042819.png]]



**<u>Summary:</u>**

<div>

|  |  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|----|
| **Time total** | **scan antivirus** | **waiting** | **json-store** | **luz-vault** | **waiting time** | **google store file** |  |
| 4872.529 ms | 259ms | 638ms | 77ms | 68ms | 3106ms | 281ms | lowest |
| 2619.299 ms | 396ms | 138ms | 73ms | 25ms | 564ms | 204ms | average |
| 1111.263 ms | 118ms | 76ms | 37ms | 12ms | 519ms | 201ms | highest |

</div>

NOTE: **The following metric data was measured on June 1, 2023, from 9:00 AM to 10:15 AM. in performance environment in Kepler team Dashboard**

**Kepler Dashboard Monitoring**: <a href="https://console.cloud.google.com/monitoring/dashboards/builder/a6c890ec-d0e3-452c-b80c-59908a2a0441;duration=PT1H?project=klara-performance" class="external-link" rel="nofollow">https://console.cloud.google.com/monitoring/dashboards/builder/a6c890ec-d0e3-452c-b80c-59908a2a0441;duration=PT1H?project=klara-performance</a>

**Metric: prometheus/application_timed_uploadDocumentToStorage_seconds/summary**


![[47390916697-image-20230601-025529.png]]



**Metric: prometheus/application_timed_createAuditData_seconds/summary**


![[47390916697-image-20230601-025702.png]]



**Metric: prometheus/application_timed_enrichDocumentListener_seconds/summary**


![[47390916697-image-20230601-024604.png]]



**Metric: prometheus/application_count_enrichDocumentListener_total/counter**


![[47390916697-image-20230601-030006.png]]



## ***2. Measure API get reference file by Id luz-docs***

***GET /luz_docs/api/{tenentId}/documents/{documentId}/files/reference***

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="4095edf5-d4c6-4423-b100-b1fecd024bd4" macro-name="view-file"><a href="../_attachments/47390916697-SendDocumentLuzDocs.html" class="confluence-embedded-file" data-nice-type="HTML Document" data-file-src="/wiki/download/attachments/47390916697/SendDocumentLuzDocs.html?version=1&amp;modificationDate=1685512671520&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/html" data-has-thumbnail="true">

![[47390916697-SendDocumentLuzDocs.html]]

</a></span>


![[47390916697-image-20230531-055604.png]]



It’s slow like which arrow team mention.

**CPU**


![[47390916697-cpu.png]]



**RAM**


![[47390916697-RAM.png]]



**Tracing**


![[47390916697-tracing.png]]



Similar to the file storage API, the file retrieval API also has a time interval where there is no request sent to the third-party server.

*Trace UR*L(Trace 1): <a href="https://console.cloud.google.com/traces/list?tid=13a715f94006a6c9a5134709f846a34a&amp;project=klara-performance&amp;pageState=(%22traceIntervalPicker%22:(%22groupValue%22:%22P2D%22,%22customValue%22:null))" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=13a715f94006a6c9a5134709f846a34a&amp;project=klara-performance&amp;pageState=("traceIntervalPicker":("groupValue":"P2D","customValue":null))</a>

Trace 2: <a href="https://console.cloud.google.com/traces/list?tid=22b1099a07f7c94b0c0f6136ccf77fad" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=22b1099a07f7c94b0c0f6136ccf77fad</a>

Trace 3:<a href="https://console.cloud.google.com/traces/list?tid=0837c79f7a5431a2e9b610493602152a" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=0837c79f7a5431a2e9b610493602152a</a>

**Summary:**

<div>

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **Time total** | **google store file** | **luz-vault** | **waiting time** | **google store file** |  |
| 10486.027 ms (Trace 1) | 80ms | 26ms | 9234ms | 590ms | lowest |
| 374.85 ms (Trace 2) | 53ms | 57ms | 175ms | 22ms | highest |
| 3484.29 ms (Trace 3) | 23ms | 80ms | 4017ms | 70ms | average |

</div>

### **<u>Guess</u>**: Since both storing and downloading documents require encryption or decryption, it is speculative to assume that this process might be causing the APIs to slow down.

Becasuse when i add metric to function to measure time run encrypt. I got time excuted increase by the time

Tracing: <a href="https://console.cloud.google.com/traces/list?tid=17de221979c083cc3ea8e97723f47cb8" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=17de221979c083cc3ea8e97723f47cb8</a>


![[47390916697-image-20230601-073041.png]]



Tracing: <a href="https://console.cloud.google.com/traces/list?tid=184e4cebd86cb36583c07e183f1e6421" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=184e4cebd86cb36583c07e183f1e6421</a>


![[47390916697-image-20230601-073428.png]]



Tracing: <a href="https://console.cloud.google.com/traces/list?tid=064ccc0aa4dec4d974439d9fc5ecaf7a" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=064ccc0aa4dec4d974439d9fc5ecaf7a</a>


![[47390916697-image-20230601-073516.png]]



So I guess the time delay between "luz-vault" and (GET, POST) requests to Google Cloud Store is due to the increased time taken for encryption and decryption processes.

Time waiting between **scan antivirus** and **json-store**, I am checking.

## **3.** ***Measure API get document meta-data by id***

**GET /luz_docs/api/{ tenant_id }/documents/{ document_id }?include-file=false**

Trace: <a href="https://console.cloud.google.com/traces/list?tid=0df2fc42ebdd9b482bc65814313fabb5" class="external-link" rel="nofollow">https://console.cloud.google.com/traces/list?tid=0df2fc42ebdd9b482bc65814313fabb5</a>


![[47390916697-image-20230602-044225.png]]



<div>

|            |                                        |         |
|------------|----------------------------------------|---------|
| **Time**   | **JsonStoreMongoDbResource.aggregate** |         |
| 587.147 ms | 536.843 ms                             | slow    |
| 695.77 ms  | 641.536 ms)                            | slower  |
| 858.818 ms | 797.859 ms)                            | slowest |

</div>

NOTE: **The following metric data was measured on June 2, 2023, from 10:00 AM to 11:00 AM. in performance environment in Kepler team Dashboard**

**Metric: prometheus/application_time_getDocumentById_seconds/summary (filtered) \[MEAN\]**


![[47390916697-image-20230602-044824.png]]



**Metric: prometheus/application_count_getDocumentMetadataByIdWithFolderNames_total/count**


![[47390916697-image-20230602-045005.png]]



**Metric: prometheus/application_time_getDocumentMetadataByIdWithFolderNames_seconds/summary**

(**getDocumentMetadataByIdWithFolderNames** is function that call aggregate json-store)


![[47390916697-image-20230602-045136.png]]



According metric and trace, I found that issue that come from **getDocumentMetadataByIdWithFolderNames**. It’s may come from query sentences.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="351537d8-4d8a-4ed0-b29b-4d98e1d7a5fb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
        "$match": {
            "$and": [
                {
                    "$expr": {
                        "$eq": [
                            {
                                "$toString": "$_id"
                            },
                            "647862867971121b9b8142f0"
                        ]
                    }
                }
            ]
        }
    },
    {
        "$lookup": {
            "from": "folders",
            "let": {
                "folderIds": "$folderIds"
            },
            "pipeline": [
                {
                    "$match": {
                        "$expr": {
                            "$in": [
                                {
                                    "$toString": "$_id"
                                },
                                "$$folderIds"
                            ]
                        }
                    }
                },
                {
                    "$project": {
                        "_id": {
                            "$toString": "$_id"
                        },
                        "name": "$name",
                        "securityClassCodes": {
                            "$concatArrays": [
                                {
                                    "$ifNull": [
                                        "$securityClassCodes",
                                        []
                                    ]
                                },
                                {
                                    "$ifNull": [
                                        "$inheritedSecurityClassCodes",
                                        []
                                    ]
                                }
                            ]
                        }
                    }
                }
            ],
            "as": "_folders"
        }
    }
```

</div>

</div>

I run in Studio 3T only for first match. It takes more than 1s


![[47390916697-image-20230607-044733.png]]



In my idea, because we try to case field \_id to string, so before make the comparison, it do more cast value, so it make query more longer.

Instead of that, we can be

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1dfc11d1-f2e1-4a19-bd0c-efd07c45b221" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
        "$match": {
            $expr: {
                $eq: [
                    "$_id",
                    ObjectId("644231b47f324f3ad06c2c6f")
                ]
            }
        }
    }
```

</div>

</div>

It takes 0.265s


![[47390916697-image-20230607-044657.png]]
