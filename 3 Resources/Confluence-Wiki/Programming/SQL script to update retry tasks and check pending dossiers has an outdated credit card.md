---
ai_hash: d8c8336f9efa13eb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 9
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48752885807/SQL+script+to+update+retry+tasks+and+check+pending+dossiers+has+an+outdated+credit+card
space: GRAVITY
status: reference
tags:
- confluence
- programming
- space/gravity
title: SQL script to update retry tasks and check pending dossiers has an outdated
  credit card
topic: programming
type: source
updated: 2025-10-15
---

# SQL script to update retry tasks and check pending dossiers has an outdated credit card

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2025-10-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48752885807/SQL+script+to+update+retry+tasks+and+check+pending+dossiers+has+an+outdated+credit+card)
> Relevance 0.731 · topic `programming`

1.  Run this script to update the task state to **DONE**  
    **SQL**:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fbedf484-5da2-4f38-9988-182b017a8b71" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    UPDATE iwa_task
    SET "State" = '6'
    WHERE taskid IN (107645530, 107778628, 107640318);
    ```

    </div>

    </div>

    **Example result:**

    

![[48752885807-image-20251015-050401.png]]



2.  Run this script to re-check the retry tasks has been **DONE**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a4b35b98-a89a-462c-b607-3382c210920b" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    SELECT taskid, caseid, "State", customvarcharfield1, name, description, customvarcharfield2, customvarcharfield3, customvarcharfield4, customvarcharfield5, starttimestamp, endtimestamp
    FROM iwa_task
    WHERE customvarcharfield1 = 'GET_ELIGIBLE_PRODUCTS_SIGNAL_TASK'
    AND starttimestamp > '2025-08-14 00:00:00'
    AND "State" != 6;
    ```

    </div>

    </div>

**Please send us the data if there are any records returned.**

3.  Run this script to check pending dossiers has old credit card (PF_MC_PREPAID_CARD_DESIGN):

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d15aa024-06c2-4609-9765-4d696e8d67a7" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    select
        json_extract_path_text(objectvalue::json, 'cobId') as cobId,
        BD.createdat,
        BD.modifiedat,
        DS.status,
        json_extract_path_text(objectvalue :: json, 'productManagement', 'products', '1') as credit_card_type
    from
        public.iwa_businessdata as BD
        left join
        public.dossier_status AS DS
        on
        json_extract_path_text(objectvalue::json, 'cobId') = DS.cobid
    where 
        BD.createdat > '2025-07-14 00:00:00.755'
        and json_extract_path_text(objectvalue :: json, 'productManagement', 'products', '1') like '%PF_MC_PREPAID_CARD_DESIGN%'
        and DS.status = 'PENDING'
        order by BD.createdat DESC
    ;
    ```

    </div>

    </div>

**Please send us the data if there are any records returned.**

Please contact “Gravity” team if you have any concerns.

%% ai-graph-start %%

**Related notes:**
- [[SQL script to investigate dossier COB003059 on INT1]]
- [[Script identify fields which are incorrectly logged in the protocol]]
- [[SQL script to investigate dossier with orphaned protocol records in PROD]]
- [[SQL script to get SOB (APP & WEB) dossier progress]]
- [[SQL script to filter IN_PROGRESS tasks more than 30 days]]

%% ai-graph-end %%