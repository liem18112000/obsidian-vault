---
title: "SQL script to search error comments of task or transaction"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47378366477/SQL+script+to+search+error+comments+of+task+or+transaction
space: "GRAVITY"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2023-05-15
attachments: 1
tags:
  - confluence
  - programming
  - space/gravity
---

# SQL script to search error comments of task or transaction

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2023-05-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47378366477/SQL+script+to+search+error+comments+of+task+or+transaction)
> Relevance 0.731 · topic `programming`

- Connect to the database of cob-cash-services-app.

- Use the below script to query some error comments in the attached log file.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9a3c87cf-5a2f-464b-b1fb-1f0055b10ba4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT COMMENT
FROM cob_task
WHERE identifier IN (
    'f9b9f721-fe38-42a8-8f98-375fb3a27553',
    '7afb09f2-7a25-4817-80e2-e8a4321a45e2',
    'ec497b3b-ee32-4c90-8444-f7bce726b359',
    '580a141b-7a88-4124-ba73-74658db9f3d3'
);
```

</div>

</div>

- Then run the script to search the comments which were errors.

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="1b1e5eb8-92d6-41ee-9a86-c086aac272ea" macro-name="view-file"><a href="../_attachments/47378366477-cob-cash-service.log" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47378366477/cob-cash-service.log?version=1&amp;modificationDate=1684126880385&amp;cacheVersion=1&amp;api=v2" data-mime-type="binary/octet-stream" data-has-thumbnail="true">

![[47378366477-cob-cash-service.log]]

</a></span>
