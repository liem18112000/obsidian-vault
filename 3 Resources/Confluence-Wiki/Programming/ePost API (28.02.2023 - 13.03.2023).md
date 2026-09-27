---
title: "ePost API (28.02.2023 - 13.03.2023)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47326069439/ePost+API+28.02.2023+-+13.03.2023
space: "LUZ"
topic: programming
relevance: 0.806
depth: 3
updated: 2023-03-13
attachments: 11
tags:
  - confluence
  - programming
  - space/luz
---

# ePost API (28.02.2023 - 13.03.2023)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-03-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47326069439/ePost+API+28.02.2023+-+13.03.2023)
> Relevance 0.806 · topic `programming`

<div class="contentLayout2">

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

## Mention

### A. OneAPI enhancement

### eBill delivery

1.  Introduce public APIs to manage logo for a biller (get/update/delete)  

    

![[47326069439-image-20230313-064118.png]]



2.  Integrate Analyze API with synchronous delivery API  

    

![[47326069439-image-20230313-031236.png]]



3.  Able to jump to next channel if Analyze API can not fulfill invoice info (payment amount, due date, qr reference)  

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

Testcase 1


![[47326069439-image-20230313-071740.png]]



*AUTO - \[DIGITAL, SMS, EBILL, PHYSICAL, EMAIL\]*

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

Testcase 2


![[47326069439-image-20230313-072000.png]]



</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

### Asynchronous delivery API with Pub/Sub

- Handle chaos exception

- Optimize thread pools

- Solved duplicated message issue

### B. Add metaspace config to each module

- luz-sms

- luz-public-api-adapter

- luz-tenant-dir

- luz-eletter

## DEMO

### Monitoring page for Admin

- Show single deliveries by admin’s selection in order descending of delivery date and time


![[47326069439-image-20230313-041015.png]]



</div>

</div>

</div>

</div>
