---
title: "ePost Forced Onboarding & One API (13.08.2024 - 26.08.2024)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47999058768/ePost+Forced+Onboarding+One+API+13.08.2024+-+26.08.2024
space: "LUZ"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2024-08-26
attachments: 3
tags:
  - confluence
  - programming
  - space/luz
---

# ePost Forced Onboarding & One API (13.08.2024 - 26.08.2024)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-08-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47999058768/ePost+Forced+Onboarding+One+API+13.08.2024+-+26.08.2024)
> Relevance 0.731 · topic `programming`

1/One API

- Intergrate Zone MTA for email deliveries (In Progress)

- Discuss and apply Backpressure for luz-eletter when storing document onto luz-docs

- Fix Bug: Delivery stuck in processing - 401 returns from luz-cache

- Increase retry delay time for delivery status message to 600s: [Load test: Increase retry delay time of \<env\>-delivery-status-luz-eletter-topic queue](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47992799633/Load+test+Increase+retry+delay+time+of+env+-delivery-status-luz-eletter-topic+queue)

2/ ePost Forced Onboarding

- Fix Bug: Expiration Cron Job update - incorrect condition (recipientTrackingId is not unique)

- Fix Bug: onboarding_process_tracking table not updated when eLetter is delivered  

  

![[47999058768-image-20240826-031830.png]]



- Fix Bug: email matching case sensitive

- Add new trigger points for forced onboarding eletter: when user activate sender activation flag  

  

![[47999058768-image-20240826-063500.png]]



<span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size">[[47999058768-scrnli_8_26_2024_10-14-26 AM.mp4|scrnli_8_26_2024_10-14-26 AM.mp4]]</span>

3/ Ivy migration

- Test and fix found Bugs on:

  - Online Dashboard

  - Accounting and Inventory

  - Insurance Dashboard

  - Internal Marketing Letter

  - One API monitoring GUI
