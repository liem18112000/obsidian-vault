---
ai_hash: 74207189663a8003
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46928857382/Public+API+30+August+2021
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Public API (30 August 2021)
topic: programming
type: source
updated: 2021-08-30
---

# Public API (30 August 2021)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-08-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46928857382/Public+API+30+August+2021)
> Relevance 0.731 · topic `programming`

A. MENTION

1.  Store metadata in eletter to track delivery

2.  Provide an API in luz_tenant_dir that return all tenants with **allow_digital_letter_box** is on

3.  Add extra flag **displayDeliveredDocument** in delivery API to allow sender can receive information of documents that were delivered successfully  

    

![[46928857382-image-20210830-041455.png]]



    

![[46928857382-image-20210830-041255.png]]



4.  Provide a new delivery API that runs **synchronously**. Limit a maximum number of recipients is 50.  

    

![[46928857382-image-20210830-044450.png]]



5.  Send PO box information to EIRENE to be verified along with address info  

    

![[46928857382-image-20210830-044313.png]]



6.  Prepare tenant directory to support multi credentials (addresses, emails, mobiles,…)

7.  Add verify score for each credential

%% ai-graph-start %%

**Related notes:**
- [[Public API Eletter (0.02.09.00)]]
- [[ePost API (28.02.2023 - 13.03.2023)]]
- [[Copy 5. How to extend modify ONE API delivery API Research]]
- [[5. How to extend modify ONE API delivery API Research]]
- [[ePost Forced Onboarding & One API (13.08.2024 - 26.08.2024)]]

%% ai-graph-end %%