---
title: "SQL script to find all documents from Fidentity"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48113188867/SQL+script+to+find+all+documents+from+Fidentity
space: "GRAVITY"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2024-10-24
attachments: 2
tags:
  - confluence
  - programming
  - space/gravity
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
