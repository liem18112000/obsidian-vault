---
ai_hash: 2f20e7ade6fefa94
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 2.73
entities: []
relevance: 0.806
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47524675593/SmartSend+Architecture
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: SmartSend Architecture
topic: architecture
type: source
updated: 2023-10-18
---

# SmartSend Architecture

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-10-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47524675593/SmartSend+Architecture)
> Relevance 0.806 · topic `architecture`

Smart Send is a firebase application relying heavily on firestore - an event enabled document database. High level components include

- Hosting (Firebase)

- Frontend Functions (Cloud Functions)

- Services (App Engine)


![[47524675593-image-20231018-063443.png]]



Alternative approach not using Firestore and keeping event driven architecture


![[47524675593-SmartSend Components2.png]]

%% ai-graph-start %%

**Related notes:**
- [[Architecture]]
- [[Unified architecture overview]]

%% ai-graph-end %%