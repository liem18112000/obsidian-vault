---
title: "[OneAPI] Architecture overview"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47144370973/OneAPI+Architecture+overview
space: "LUZ"
topic: architecture
relevance: 0.81
depth: 2.67
updated: 2025-03-04
attachments: 34
tags:
  - confluence
  - architecture
  - space/luz
---

# [OneAPI] Architecture overview

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-03-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47144370973/OneAPI+Architecture+overview)
> Relevance 0.81 · topic `architecture`

<div>

|  |  |
|----|----|
| Creation Date | 11 Jul 2022 |
| Status | <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-complete conf-macro output-inline" hasbody="false" macro-id="1301eb77-97d2-4422-a14d-d836f4751474" macro-name="status">IN PROGRESS</span> |
| Related Epic(s) | <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47144370973_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-81084" macro-id="9e865e23-3f7f-434f-941c-c0529e8e7966" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-81084" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-81084</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47144370973_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-78211" macro-id="ad65c9d1-6b16-4140-9846-7a858b952e37" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-78211" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-78211</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> |
| Related Documentation | [One API Squad](https://axonivy.atlassian.net/wiki/spaces/SQUAD/pages/47110914261/One+API+Squad) [One API - Delivery channels dispatching](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47121007316/One+API+-+Delivery+channels+dispatching) [KLARA physical letterbox](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47120154765/KLARA+physical+letterbox) [Identity Matching](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519060000/Identity+Matching) [E-Letter Delivery Flow](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530458151/E-Letter+Delivery+Flow) |

</div>

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="4daa6b5c-5d74-4886-9f6b-dd095b78c9c9" macro-name="toc">

</div>

# Overview

The following modules overview show how the internal architecture behind the OneAPI


![[47144370973-OneAPI - Architecture overview.png]]



# Delivery Tracking

<span class="inline-comment-marker" ref="032fe5b1-9662-4be6-a5d5-1261414ffec0">Below is the delivery tracking flow for SMS channel, but the idea is general and can be applied for the other delivery channels</span>.

## ONE API - Delivery process flow on luz-eletter

<span class="confluence-embedded-file-wrapper image-center-wrapper"><img src="https://axonivy.atlassian.net/wiki/download/attachments/20530458151/Delivery%20v2%20asynchronously.png?version=60&amp;modificationDate=1661318969316&amp;cacheVersion=1&amp;api=v2" class="confluence-embedded-image image-center" loading="lazy" data-image-src="https://axonivy.atlassian.net/wiki/download/attachments/20530458151/Delivery%20v2%20asynchronously.png?version=60&amp;modificationDate=1661318969316&amp;cacheVersion=1&amp;api=v2" data-base-url="https://axonivy.atlassian.net/wiki" /></span>

## Delivery channel tracking flow


![[47144370973-SMS delivery channel tracking.png]]



<div id="expander-699899585" class="expand-container conf-macro output-block" hasbody="true" macro-id="6ebdf020-0880-46f7-9626-297d9cfb69b0" macro-name="expand">

<div id="expander-control-699899585" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">sms_tracking table structure in luzsms database, public schema</span>

</div>

<div id="expander-content-699899585" class="expand-content expand-hidden">

<div>

|  |  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|----|
| **… id, create_by, create_date, update_date, update_by** | **amount (int4 not null)** | **billed (boolean not null)** | **tenant_id (varchar 255 not null)** | **company_id (int8 not null)** | **origin (varchar 255)** | **origin_href (varchar 255)** | **cost_center (varchar 255)** |
|  | 1 | false | 6eace38d-dad1-4487-8a30-814bd50ee6b1 | 1 | OneAPI | epost/v2/deliveries/3/status | MARKETING |
|  |  |  |  |  |  |  |  |

</div>

</div>

</div>

Given a record in the sms_tracking table in luzsms database, we can back reference using the origin and origin_href.

Given the example record in the table above, with the origin and origin_href: **epost/v2/deliveries/3/status**, we know that this sms is sent via the **OneAPI**, luz-eletter, and which tenant sent it.  
Then we can head to luz-eletter database, look for that tenant schema, look inside the documents table and delivery table, and look for the records that have the same delivery id to know more details about that delivery and its documents.

# Dispatching logic - Channel preference: AUTO

When the sender leaves the delivery channel preferences empty, or enters “AUTO” as the channel preference, KLARA will use the pre-defined channels order (mentioned in <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47121007316/One+API+-+Delivery+channels+dispatching#Delivery-channels-usage" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47121007316/One+API+-+Delivery+channels+dispatching#Delivery-channels-usage</a> ).  
The diagrams below describe the logic behind it through some example delivery data.

The document and metadata (every recipient level) will go through 2 steps below:  
**Filter step:** check validates the metadata and files against each delivery channel’s set of rules. The output of this check is a list of eligible channels for this document to proceed further.  
**Sending step:** try to send with the eligible channels as orders until one channel was a success or all failed.

The UML diagram shows how the **Filter** job is designed:

<span class="confluence-embedded-file-wrapper image-center-wrapper"><img src="https://axonivy.atlassian.net/wiki/download/attachments/47150563870/Dilivery%20channel%20rule%20-%20class%20diagram.png?version=3&amp;modificationDate=1660186726772&amp;cacheVersion=1&amp;api=v2" class="confluence-embedded-image image-center" loading="lazy" data-image-src="https://axonivy.atlassian.net/wiki/download/attachments/47150563870/Dilivery%20channel%20rule%20-%20class%20diagram.png?version=3&amp;modificationDate=1660186726772&amp;cacheVersion=1&amp;api=v2" data-base-url="https://axonivy.atlassian.net/wiki" /></span>

The diagram shows how the **Sending** job is designed:


![[47144370973-UML Sending Letter.png]]




![[47144370973-AUTO preference - Dispatching logic.png]]



# <span class="inline-comment-marker" ref="3505312b-d175-4722-b826-8b257da9b370">Updated architecture to handle large delivery</span>

## Overview


![[47144370973-Animation.gif]]



## Storing documents from sender

When we receive the documents from the sender, we need to be able to store them first before returning a delivery tracking id to the sender so the sender can start tracking the status of the delivery.

The speed of this storing process needs to be acceptable so that the sender does not have to wait for too long, but we also need to limit the resources used for this process so that the system is not overloaded when multiple senders send us deliveries at the same time.

A thread pool could be utilized to speed up the process while limiting the resources spent:


![[47144370973-storing-document.png]]



**Open point: should we make sure that all the documents are successfully received and stored before returning the delivery tracking id back?**   
**Should the storing process be improved?**   
**Or should we just define a limit on how many documents we can receive and store?**  
One idea is we could split the large delivery to small batches of documents and redirect each batch to different instance of luz-eletter, so that the large delivery could be stored faster.

## Pushing documents info to Sending queue

After we are able to secure the documents received from the sender, we can start enqueue those documents to Google PubSub. This enqueue process also needs to be reasonably fast and should use up a limited resources.


![[47144370973-document-sending.png]]



What happens when during the sending documents to Google PubSub process, an unexpected issue happens, and some documents are not enqueued?

A kubernetes cronjob might be needed to pick up the documents (that are not yet enqueued from luz-eletter database to continue the enqueue process for those documents.


![[47144370973-document-sending-retry.png]]



## Process documents info from Sending queue

After the documents are enqueued in Google PubSub, the document delivering process starts:


![[47144370973-process-documents.png]]



**Open point: What happens if after acknowledging document info from Sending queue, and the pod dies before it can send document info to Document status queue?**

## Update document status from Document status queue

After the delivering process for the documents are completed, and the document statuses are available in the Document status topic, the update process for those document statuses starts:


![[47144370973-document-status.png]]



Should the dispatching logic (pulling documents from Sending queue and deliver) and the update document status process (pulling documents statuses)

## Update delivery status

When all the documents of a delivery complete the delivery process, the delivery’s status finally gets updated:


![[47144370973-delivery-status-update.png]]



**~~Should we rely on cronjob to update delivery status?~~**  
Delivery status might be updated a bit later even if its documents have already completed the delivery process and have statuses, sender might need to wait for a bit longer.

~~Other approach:~~

- **~~only calculate the delivery status when a client request it (only if the delivery status is still in PROCESSING status).~~**

## <span class="inline-comment-marker" ref="3eca4c95-126f-4047-b096-ab627c77aa44">Small/Large document batches</span>

If we need to support this point, then we can setup 1 topic and 3 dispatchers, each dispatcher will pull and process the document messages that belongs to the appropriate delivery size (Small, Medium, Large),  
when luz-eletter receives the documents from the sender, it knows the delivery size of each delivery, so it will assign each document with a deliverySize attribute and publish the message to the Sending queue.


![[47144370973-oneapi-pubsub-smalllarge.png]]

![[47144370973-image-20220926-100004.png]]



In the latest implementation, we add the configuration parameter MAX_NO_RECIPIENT_FOR_SMALL_DISPATCHER=1000 to define which deliveries are small and which deliveries are large. And we also define different lanes for small and large deliveries to avoid the case that the large deliveries block the small ones.

## Synchronous API

There are 2 types of oneAPI, one is the asynchronous type where the sender after sending the documents through receive a delivery tracking id to track its status, the other type is synchronous where the sender after sending the documents through have to wait until the whole delivery is completed and receive the whole delivery’s status.

The synchronous oneAPI still needs to be supported after we utilize Google PubSub.  
Google PubSub does not seem to support synchronous behavior where the Publisher publish a message and wait until a result is returned by Google PubSub.

Therefore if we want to use Google PubSub for the synchronous API to hopefully reduce the maintenance effort, we would need to find a way to keep the connection from the sender to KLARA opened while the documents are being processed with Google PubSub, then after all the documents are done, we can then return the whole delivery status to the sender and close the connection. (**Still need investigation to see if this is possible**).

If the above is not possible, we might still need to keep the current implementation that we have today for the synchronous API (using thread pools to keep the synchronous api’s performance acceptable while limiting the resources spent).

## Resilience

oneAPI is expected to serve relatively big senders sending upto a million documents.  
Therefore it is important that oneAPI is reliable and resilient.

To achieve those goals, we need to know what is the limit of oneAPI? What are the internal modules capable of handling? What happens if the system reaches those limits? It should be able to protect itself and prevent a total system crash.

We will need to assess each module to know its limits and based on that, define the limit of oneAPI.  
And to protect the system from crashing when the limit is reached, the **Bulkhead pattern** and **Backpressure technique** should be utilized.

For backpressure, we could research to see if we could configure at the Kubernetes/GCP level or not so that when a threshold for cpu/memory/requests is reached, Kubernetes will prevent further requests from reaching the modules,  
an internal load balancer could be setup to frequently request to the health check endpoint of each module,  
the internal load balancer can measure how long the health check api takes to response, if the request takes too long, then it is a indication that the modules are under heavy load,  
or the health check endpoint will gather some info (we can define what info should be considered, for example: number of processing documents, if larger than the threshold, then the module is at its limit) and decide if this module can receive more requests or not.

For horizontally scaling, number of requests should be considered instead of cpu/memory because cpu/memory might be spike as high to trigger the horizontal scale action even though the module is under heavy load.

## Next steps

1.  Handle storing of documents before sending back delivery tracking id

2.  Publish documents info to Sending queue in Google PubSub after storing sender’s documents

3.  Pull documents info from Google PubSub’s Sending queue to dispatcher to deliver documents

4.  Publish single delivery for each recipient in Google PubSub  

    

![[47144370973-image-20221110-071136.png]]



5.  Pull single delivery message from the queue and process it

6.  Update **document status**

7.  Update **delivery status** <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47144370973_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-89096" macro-id="20c388e8-ba6f-4dbe-9595-00292e9205d2" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-89096" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-89096</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

8.  <span class="inline-comment-marker" ref="4b4af8d1-aa9a-40ac-8eff-3b77aff12269">Handle the </span>**<span class="inline-comment-marker" ref="4b4af8d1-aa9a-40ac-8eff-3b77aff12269">retry </span>**to enqueue documents that were not yet enqueued <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47144370973_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-89887" macro-id="8703c3ef-3b63-462a-b369-8b5185a6d666" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-89887" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-89887</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

9.  Research and apply **bulkhead** and **backpressure** <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47144370973_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-89888" macro-id="e118a1db-80d2-444a-b4ae-d8b09386e8b0" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-89888" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-89888</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

10. **Load test** the whole system and figure out the system’s **limit** and what happens when the limit is reached  
    10.1 Handle **503** response in case the server cannot serve any coming requests.

11. Apply Pub/Sup for the channel delivery step (**consider if needed)**  

    

![[47144370973-image-20221110-071006.png]]



12. Apply **caching** to prevent sending many calls to luz_docs to get documents (consider after having performance testing results)  

    

![[47144370973-image-20221110-071646.png]]



13. **Database** access: DONE  
    Currently, with single document delivery, we have a few steps to handle it in luz_eletter in which we make calls to the database to update document status:  
    - Store delivery info in the delivery table (1) - PROCESSING  
    - Store document info in the document table (1) - PREPARED  
    - Store recipient info in the recipient tracking table (1) - NULL  
    - Do identity matching for each document (n) → consider doing matching for the whole delivery  
    - Update document status after store temporary in sender folder (n) - STORED_TO_SERNDER/FAILED_TO_STORE  
    - Process sending document topic and update status (n ) - PUBLISHED_TO_TOPIC  
    - Update status of single recipient delivery (n \* m)  
    - Update status for document (n)  
    - Update status for delivery (1)  
    - Update billing consumption info (1?)  
    So we can see that there are many calls to DB are made. We should find a solution to avoid spam DB.  
    **Actions**:  
    a. Pull multi messages from the recipient status topic queue and use batch updates instead of single updates one by one.  
    b. Should try out for message pulling batch and batch update/insert into DB then we can consider using “update recipient topic” for updating document and delivery status instead of introduce 2 new topic

14. luz_address_normalizer : bundle data for normalizing requests from the delivery flow  

    

![[47144370973-image-20221110-094827.png]]



15. <span class="inline-comment-marker" ref="e8cd1fa7-52a3-43f2-8cf5-c66a47bad7e5">Update the identity matching step</span>:  
    - Bundle identity matching requests (in delivery scope)  
    - Store identity matching result in tenant_dir then delivery flow can refer to if need.  
    - Consider to reuse matching flow for each delivery  
    <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47144370973_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-96033" macro-id="c4b09c56-bb88-41f5-99f6-f97bd0917b02" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-96033" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-96033</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

16. Sender blacklist information: filter out the blacklist information to make sure only related data is available for the correspondence recipient

17. **Set TimeOut** for Recipient-Consumer of the recipient topic level  
    Apply Fault Tolerance

18. Prioritize lanes for small/large deliveries: DONE

19. jsonstore only allows 10MB request so we cannot store document with metadata larger than that, then we should not store
