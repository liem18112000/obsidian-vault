---
ai_hash: 5c77f2aec5c8e9ae
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48113188867/SQL+script+to+find+all+documents+from+Fidentity
space: GRAVITY
status: reference
tags:
- confluence
- programming
- space/gravity
title: SQL script to find all documents from Fidentity
topic: programming
type: source
updated: 2024-10-24
---

# SQL script to find all documents from Fidentity

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2024-10-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48113188867/SQL+script+to+find+all+documents+from+Fidentity)
> Relevance 0.731 · topic `programming`

- Connect to the database of cob-unattended-services-app

- Run the SQL script below to find all documents related to the dossierId = 'a02a29a5c83d47e69f2a2d6dc9f8af45'

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4e35d430-4c24-487b-a3ab-fce7af0a44e2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
select
    identifier,
    fidentity_url_identifier,
    external_reference,
    "type" ,
    media_type ,
    uri
from
    cob_fidentity_document_uri
where
    fidentity_url_identifier = '80c7d82ce89649daa44334e1aa5846c2'
```

</div>

</div>

- After running the script above, the result will be similar to the image below.


![[48113188867-image-20241024-074602.png]]

%% ai-graph-start %%

**Related notes:**
- [[SQL script to get SOB (APP & WEB) dossier progress]]
- [[SQL script to investigate dossier COB003059 on INT1]]
- [[SQL script to investigate dossier with orphaned protocol records in PROD]]
- [[SQL script to search error comments of task or transaction]]
- [[SQL script to filter IN_PROGRESS tasks more than 30 days]]

%% ai-graph-end %%