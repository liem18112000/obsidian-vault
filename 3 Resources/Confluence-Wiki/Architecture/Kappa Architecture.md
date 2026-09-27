---
title: "Kappa Architecture"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457109088/Kappa+Architecture
space: "AI"
topic: architecture
relevance: 0.721
depth: 2.37
updated: 2017-07-14
attachments: 1
tags:
  - confluence
  - architecture
  - space/ai
---

# Kappa Architecture

> [!info] Imported from Confluence
> Space **AI** · updated 2017-07-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457109088/Kappa+Architecture)
> Relevance 0.721 · topic `architecture`

Kappa Architecture is a simplification of [Lambda Architecture](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457109074/Lamda+Architecture). A Kappa Architecture system is like a Lambda Architecture system with the batch processing system removed. To replace batch processing, data is simply fed through the streaming system quickly.

The canonical data store (global truth) in a Kappa Architecture system is an append-only immutable log. From the log, data is streamed through a computational system and fed into auxiliary stores for serving.


![[2457109088-kappa.png]]



## Critism

TBD.

## See also

<a href="http://kappa-architecture.com" class="external-link" rel="nofollow">http://kappa-architecture.com</a>
