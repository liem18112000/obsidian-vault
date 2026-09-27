---
ai_hash: bce7ab59f184c5f0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47919595521/SQL+script+to+filter+IN_PROGRESS+tasks+more+than+30+days
space: GRAVITY
status: reference
tags:
- confluence
- programming
- space/gravity
title: SQL script to filter IN_PROGRESS tasks more than 30 days
topic: programming
type: source
updated: 2024-07-08
---

# SQL script to filter IN_PROGRESS tasks more than 30 days

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2024-07-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47919595521/SQL+script+to+filter+IN_PROGRESS+tasks+more+than+30+days)
> Relevance 0.731 · topic `programming`

- Connect to the database of cob-services-app

- Run the SQL script below to filter all the **IN_PROGRESS** compliance tasks that have their **last changed date over 30 days** from **now**.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="71b76b50-a82f-4392-bee3-0a9eefcc6eb0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT
    CT.IDENTIFIER,
    CT.dossier_id,
    CT.status,
    EXTRACT(DAY FROM NOW()::timestamp - CT.LAST_CHANGE_DATE::timestamp) AS date_interval
FROM
    PUBLIC.COB_TASK CT
WHERE
    CT.STATUS = 'IN_PROGRESS'
AND
    EXTRACT(DAY FROM NOW()::timestamp - CT.LAST_CHANGE_DATE::timestamp) >= 30;
```

</div>

</div>

- After running the script above, the result will be similar to the image below.

  

![[47919595521-image-20240708-034127.png]]

%% ai-graph-start %%

**Related notes:**
- [[SQL script to search error comments of task or transaction]]
- [[SQL script to get SOB (APP & WEB) dossier progress]]
- [[SQL script to investigate dossier COB003059 on INT1]]
- [[SQL script to update retry tasks and check pending dossiers has an outdated credit card]]
- [[SQL script to investigate dossier with orphaned protocol records in PROD]]

%% ai-graph-end %%