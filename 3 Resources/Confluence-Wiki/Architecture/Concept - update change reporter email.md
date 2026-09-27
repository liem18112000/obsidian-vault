---
title: "Concept - update/change reporter email"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47220164020/Concept+-+update+change+reporter+email
space: "LUZ"
topic: architecture
relevance: 0.711
depth: 2.44
updated: 2022-11-28
attachments: 2
tags:
  - confluence
  - architecture
  - space/luz
---

# Concept - update/change reporter email

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-11-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47220164020/Concept+-+update+change+reporter+email)
> Relevance 0.711 · topic `architecture`

### I. Enabled email field when create reporter from employee


![[47220164020-image-20221124-040408.png]]



Enabled email input when add time reporter from employee

### II. Update email reporters

We got a little trouble when update email reporters because email reporters linked to users and reporter audit (calculate billing). we rename email of user login and rename email for reporter manager

In order to update email from <a href="mailto:reporterA@axonivy.io" class="external-link" rel="nofollow">reporterA@axonivy.io</a> to <a href="mailto:reporterB@axonivy.io" class="external-link" rel="nofollow">reporterB@axonivy.io</a>, we need to adapt

1.  reporter audit:

    1.  Add more field: time_reporter_id

    2.  Calculate billing reporter, group by time_reporter_id

    3.  Handle action change email

2.  Users login

    1.  Admin needs to handle by hand to reduce risks that mean the admin adds <a href="mailto:reporterB@axonivy.io" class="external-link" rel="nofollow">reporterB@axonivy.io</a> into the system.  

        

![[47220164020-image-20221128-090055.png]]



3.  Reporter manager

    1.  If reporterA is reporter manager, will be rename email to <a href="mailto:reporterB@axonivy.io" class="external-link" rel="nofollow">reporterB@axonivy.io</a>

### III. Scenario allow update email reporters

.
