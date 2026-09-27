---
title: "Retry for storing documents from One API to luz_docs_view_controller"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47453503750/Retry+for+storing+documents+from+One+API+to+luz_docs_view_controller
space: "LUZ"
topic: programming
relevance: 0.818
depth: 3
updated: 2023-08-28
attachments: 8
tags:
  - confluence
  - programming
  - space/luz
---

# Retry for storing documents from One API to luz_docs_view_controller

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-08-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47453503750/Retry+for+storing+documents+from+One+API+to+luz_docs_view_controller)
> Relevance 0.818 · topic `programming`

From One API, we have to call 2 times for storing documents at two separated steps

- For each steps, we need to apply retry mechanism and specific at least two values (delay and max retries)

1.  Step 1 - storing document in sender folder  
    - **delay**: 5000 ms  
    - **max retries**: 3 times  
    - **implementation**: using annotation @Retry

    1.  Store only metadata: `ch.klara.luz.eletter.services.DocumentStorageService.storeOnlyDocumentMetadata`

        

![[47453503750-image-20230808-035950.png]]



    2.  Store document and metadata: `ch.klara.luz.eletter.services.DocumentStorageService.storeDocumentTempWithRetry`

        

![[47453503750-image-20230808-040112.png]]



2.  Step 2 - storing document in recipient folder

From luz-eletter-dispatcher call to → luz-docs-view-controller (ch.klara.luz.eletter.rest.client.LuzDocsViewControllerRestClient.storeDocument)

- Max Retries: 2

- Delay: 120s

That policy is currently being hardcoded in this class: ch.klara.luz.eletter.services.dispatcher.DigitalChannelDispatcher.processToSendDocument

(skip retrying when multiple recipient matched case happens )


![[47453503750-image-20230815-064853.png]]
