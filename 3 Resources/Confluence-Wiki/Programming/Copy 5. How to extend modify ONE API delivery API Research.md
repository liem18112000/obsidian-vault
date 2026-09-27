---
ai_hash: 713fc871c5877d52
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 27
depth: 2.6
entities: []
relevance: 0.775
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49307550227/Copy+5.+How+to+extend+modify+ONE+API+delivery+API+Research
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Copy 5. How to extend/modify ONE API delivery API Research
topic: programming
type: source
updated: 2026-04-08
---

# Copy 5. How to extend/modify ONE API delivery API Research

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-04-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49307550227/Copy+5.+How+to+extend+modify+ONE+API+delivery+API+Research)
> Relevance 0.775 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="0c5353aa-7399-4be4-8333-99a2b17d6f36" macro-name="toc">

</div>

# I. Introduce external status for delivery

**- Problem statement:**

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49307550227_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-114446" macro-id="662f22c8-6a21-4cbe-a011-756b46020d8c" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-114446" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-114446</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>  
Currently, get delivery status Public API and DeliveryEntity are using the same status class. Therefore, any modification to the status would require our third parties to adapt accordingly.  
=\> Introduce an external status enum for get delivery status Public API. After this, we can add new status for DeliveryEntity such as FORCED_ONBOARDING_RECIPIENTS, etc …  
(Currently get delivery status Public API is also using a external status for DocumentEntity)  
**- Idea:**  
Introduce new module to handle the logic, and only call luz-eletter to execute  
Introduce new `ExternalDeliveryStatus` for get delivery status Public API  
Add new status in `DeliveryStatus` to indicate epostForcedOnboarding  
This new delivery status is a completion status(treat it like a completion case)

**- How:**

1/ Modify the get delivery status Public API.

- When delivery status is new status, the getStatusApi return PROCESSING_STATUS.

- When document status is new status, the getStatusApi return the document in pendingDocument array.

# II. Introduce new internal status for delivery entity

**Concept**:  
How to identify a delivery have non match recipients:


![[49307550227-CONCEPT.png]]



**Current Implementation of Digital channel with pubSub.**


![[49307550227-PubSub.png]]



# **III. Delivery Flow extended state diagram:**

**RecipientStateDiagram:**


![[49307550227-newRecipientDiagram.png]]



**DocumentStateDiagram:**


![[49307550227-Delivery document state diagram.png]]



**DeliveryStateDiagram:**


![[49307550227-Delivery state diagram.png]]



# **III. Implementation**

**1/ Modifications:**

Introduce external status concept.

Introduce new internal statuses:

- Recipient level: `RecipientTrackingStatus.ONBOARDING`

- Document level: `DocumentDeliveryStatus.RECIPIENTS_ONBOARDING`

- Delivery level: `DeliveryStatus.RECIPIENTS_ONBOARDING`

We need to modify current updating status flow for document and delivery

**2/ Extensions:**


![[49307550227-3 Process.png]]



1.  Clean up document after 2 years

2.  Delivery status set to expired after 2 years

3.  Update data for (3)

# **IV. Note**

*(1) - How to know when a delivery finishes*


![[49307550227-image-20240219-080225.png]]



  
*(2) - How to know which delivery should we trigger forced onboarding*

- Delivery status is new status

- Forced onboarding body is included in metadata

  
(3) - what kind of data should we store the in this step to resend document later on

- deliveryId, documentId, recipientsEmail, senderTenantId, companyId, recipientTrackingId, status(email registered or not)…

\(4\) - copyDocumentToRecipientFolder


![[49307550227-image-20240219-075108.png]]

![[49307550227-image-20240219-075337.png]]



  
  
Concern: luz-doc sent folder always save document when all recipients are non-match  
After user registered email can we point to some step to continue delivery

V. Continue research: How luz-eletter update status of recipients, documents & deliveries  
3 thing that affect the statuses: 3 message queue

1/ Recipients status are updated when `proceedSingleSendingDocumentToRecipient` only two kind of status Fail or Successful, along with ErrorCode. Question: should we add new status


![[49307550227-image-20240221-073741.png]]




![[49307550227-image-20240221-073158.png]]



- 3 ErrorCode.NON_MATCH_RECIPIENT:

  

![[49307550227-image-20240221-044716.png]]



  1\. Digital channel  
  2. Ebill Channel  
  3. allRecipientsRequired flag(SPECIAL_CASE)

%% ai-graph-start %%

**Related notes:**
- [[5. How to extend modify ONE API delivery API Research]]
- [[One API 0.03.28.00 (11.08.2026 - 24.08.2026)]]
- [[ePost Forced Onboarding & One API (13.08.2024 - 26.08.2024)]]
- [[Research The concept to update the status of delivery instantly after all documents are processed]]
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]

%% ai-graph-end %%