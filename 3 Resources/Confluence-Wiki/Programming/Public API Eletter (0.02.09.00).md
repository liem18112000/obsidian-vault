---
ai_hash: 4677919ad87c8d95
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.57
entities: []
relevance: 0.746
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47011137011/Public+API+Eletter+0.02.09.00
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Public API/Eletter (0.02.09.00)
topic: programming
type: source
updated: 2021-12-06
---

# Public API/Eletter (0.02.09.00)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-12-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47011137011/Public+API+Eletter+0.02.09.00)
> Relevance 0.746 · topic `programming`

A. Mention

1.  Process token from QR code

2.  Integrate with APIs from Optimus to add/update pinned branded folder to luz-profile:  

    

![[47011137011-1638765029167-20211206-065045.JPEG]]



3\. Smart letter: don’t allow user answer the second time:  


![[47011137011-image-20211201-034916-20211206-034325.png]]



4\. Implement Webhook notification authentication with client ID and secret when a new letter comes in the sender letterbox.


![[47011137011-image-20211206-071804.png]]

![[47011137011-image-20211206-072441.png]]

![[47011137011-image-20211206-072518.png]]



  

5\. Fix bug that some deliveries are not processed for retrying completedly

6\. Enhance code of identity matching to prevent the case that Identity matching status update took very long

%% ai-graph-start %%

**Related notes:**
- [[Public API (30 August 2021)]]
- [[Public API eletter token (00.02.08.00)]]
- [[ePost Forced Onboarding & One API (13.08.2024 - 26.08.2024)]]
- [[ePost API (28.02.2023 - 13.03.2023)]]
- [[5. How to extend modify ONE API delivery API Research]]

%% ai-graph-end %%