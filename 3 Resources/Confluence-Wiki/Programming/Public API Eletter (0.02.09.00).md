---
title: "Public API/Eletter (0.02.09.00)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47011137011/Public+API+Eletter+0.02.09.00
space: "LUZ"
topic: programming
relevance: 0.746
depth: 2.57
updated: 2021-12-06
attachments: 6
tags:
  - confluence
  - programming
  - space/luz
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
