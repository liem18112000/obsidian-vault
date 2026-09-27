---
title: "SmartSend Architecture"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47524675593/SmartSend+Architecture
space: "LUZ"
topic: architecture
relevance: 0.806
depth: 2.73
updated: 2023-10-18
attachments: 5
tags:
  - confluence
  - architecture
  - space/luz
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
