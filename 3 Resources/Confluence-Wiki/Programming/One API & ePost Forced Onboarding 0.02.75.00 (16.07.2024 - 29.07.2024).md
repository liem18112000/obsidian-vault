---
title: "One API & ePost Forced Onboarding 0.02.75.00 (16.07.2024 - 29.07.2024)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47950660315/One+API+ePost+Forced+Onboarding+0.02.75.00+16.07.2024+-+29.07.2024
space: "LUZ"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2024-07-29
attachments: 16
tags:
  - confluence
  - programming
  - space/luz
---

# One API & ePost Forced Onboarding 0.02.75.00 (16.07.2024 - 29.07.2024)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-07-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47950660315/One+API+ePost+Forced+Onboarding+0.02.75.00+16.07.2024+-+29.07.2024)
> Relevance 0.738 · topic `programming`

### 1. One API:

1.  Set number of documents/recipients for a delivery to be considered large to 100 (Bulk deliveries)

2.  Integration with zone-MTA

    1.  Call zone MTA to send email with feature switch (finish on our side)

    2.  Handle sync and async email deliveries process to new zone MTA (finish on our side)

    3.  Introduce public api for zone MTA to return email delivery response (finish on our side)

3.  Physical channel: Allow sender to config the sender address that will be printed on letter.

    

![[47950660315-bbbfacac-4f9a-4658-b20a-c90ad6640e49#media-blob-url=true&id=4cd01009-dc45-4f41-b.png]]



### 2. ePost Forced Onboarding:

1.  Link forced onboarding eletter delivery to onboarding request email delivery

2.  Set forced onboarding eletter to fail if onboarding request email is failed to send (in-progress)


![[47950660315-onboardingLink.png]]



### 3. News Manager: 

Removal Klara Regional: No longer synchronise News Posts to Klara regio  

1.  Removal Klara Regional: No longer synchronise data changes in workplace and online shop to Klara regio  

    <span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[47950660315-scrnli_7_29_2024_11-07-57 AM.mp4|scrnli_7_29_2024_11-07-57 AM.mp4]]</span>

2.  Removal Klara Regional: Remove references to regio from Klara Online Dashboard  

    

![[47950660315-image-20240729-034914.png]]

![[47950660315-image-20240729-034957.png]]

![[47950660315-image-20240729-035041.png]]



    

![[47950660315-image-20240729-035209.png]]



3.  Removal Klara Regional: Remove all regio deal posts  

    

![[47950660315-image-20240729-035317.png]]



      

    

![[47950660315-image-20240729-035448.png]]

![[47950660315-image-20240729-035846.png]]



4.  Remove Klara Regional: No longer synchronise with Klara regio when the tenant create or update an online profile (in progress)
