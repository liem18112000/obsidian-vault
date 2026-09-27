---
title: "SQL script to investigate dossier COB003059 on INT1"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48571645980/SQL+script+to+investigate+dossier+COB003059+on+INT1
space: "GRAVITY"
topic: programming
relevance: 0.786
depth: 3
updated: 2025-07-08
attachments: 7
tags:
  - confluence
  - programming
  - space/gravity
---

# SQL script to investigate dossier COB003059 on INT1

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2025-07-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48571645980/SQL+script+to+investigate+dossier+COB003059+on+INT1)
> Relevance 0.786 · topic `programming`

1.  Run this script to find the dossiers' status.  
    **Query**:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e1dbaf31-1c66-4243-982f-d5ddf279dde8" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    select * from dossier_status where cobId = 'COB003059'
    ```

    </div>

    </div>

    **Example result:**


![[48571645980-image-20250708-101952.png]]



2\. Run this script to find the business data

**Query:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="86536633-ee36-42c5-84a0-e82e9883d0c4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
select * 
from iwa_businessdata 
where json_extract_path_text(objectvalue :: json, 'cobId') = 'COB003059';
```

</div>

</div>

**Example result:**


![[48571645980-image-20250708-105508.png]]



3\. Run this script to find the task

**Query:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8a252b1f-9549-40e6-91c2-52c786d56dc5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
select * from iwa_task 
where customvarcharfield1 = 'GET_ELIGIBLE_PRODUCTS_SIGNAL_TASK'
and customvarcharfield2  = 'COB003059';
```

</div>

</div>

**Example result:**


![[48571645980-image-20250708-105153.png]]



Please contact “Gravity” team if you have any concerns.
