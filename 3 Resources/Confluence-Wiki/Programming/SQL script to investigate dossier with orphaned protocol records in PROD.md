---
ai_hash: 9d32ca15542c1219
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.81
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48622764072/SQL+script+to+investigate+dossier+with+orphaned+protocol+records+in+PROD
space: GRAVITY
status: reference
tags:
- confluence
- programming
- space/gravity
title: SQL script to investigate dossier with orphaned protocol records in PROD
topic: programming
type: source
updated: 2025-08-18
---

# SQL script to investigate dossier with orphaned protocol records in PROD

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2025-08-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48622764072/SQL+script+to+investigate+dossier+with+orphaned+protocol+records+in+PROD)
> Relevance 0.724 · topic `programming`

Connect to the database of AxonIvy engine of PROD environment to execute the below scripts to check if there are any protocol records with cobId bigger than current cobId.

## 1. Query current dossier counter

### Query:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ed9b86eb-ff34-4316-a5d9-ff0b682785c9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT
    businessdataid AS id,
    json_extract_path_text(objectvalue::json, 'value') AS current_number
FROM iwa_businessdata
WHERE objecttype = 'ch.axonivy.fintech.standard.dossier.DossierIdGenerator$DossierIdSeed';
```

</div>

</div>

### Example result:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ba942b5c-dfc3-4488-9814-e0509a9318d3" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
id    |current_number|
------+--------------+
COB   |49818         |
WEB   |822           |
APP   |13131         |
```

</div>

</div>

## **2. Check orphaned protocol for COB**

### Query:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c6ff320e-6f41-430b-a1af-122f25fcd712" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
WITH dossier_counter AS (
    SELECT businessdataid AS id,
        json_extract_path_text(objectvalue::json, 'value')::INTEGER AS current_cob_number
    FROM iwa_businessdata
    WHERE objecttype = 'ch.axonivy.fintech.standard.dossier.DossierIdGenerator$DossierIdSeed'
        AND businessdataid = 'COB'
),
orphaned_protocol AS (
    SELECT dossierid,
        externalreference,
        MIN(username) AS username,
        MAX("timestamp") AS timestamp
    FROM public.protocol
    WHERE dossierid LIKE 'COB%'
        AND substring(dossierid, 4, 6)::INTEGER > (SELECT current_cob_number FROM dossier_counter)
    GROUP BY dossierid, externalreference
)
SELECT orphaned_protocol.*,
    iwa_businessdata.businessdataid,
    iwa_businessdata.modifiedat
FROM orphaned_protocol
    LEFT JOIN iwa_businessdata
    ON orphaned_protocol.externalreference = iwa_businessdata.businessdataid;
```

</div>

</div>

### Example result:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d167a2e7-ab3e-4ef8-b39a-248bfec4ac15" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
dossierid                                            |externalreference               |username    |timestamp                    |businessdataid                  |modifiedat             |
-----------------------------------------------------+--------------------------------+------------+-----------------------------+--------------------------------+-----------------------+
COB050016_CO_6.1_b3f8e1ff-1082-46c8-8994-8c4f0a14e31b|0c2c8974702f44f09626947ecb7f0029|officer_thai|2025-08-18 11:10:45.010 +0700|                                |                       |
COB050016                                            |06af73360e844e35bbb592c1b590c2c0|kube_thai   |2025-08-18 11:13:01.621 +0700|06af73360e844e35bbb592c1b590c2c0|2025-08-18 06:13:04.978|
COB050016_CO_6.1_e6d138c0-05d3-4297-858f-9b8d707cb1e3|3937a037c41042f29ac95316782aa370|officer_thai|2025-08-18 11:09:31.191 +0700|                                |                       |
COB050016                                            |                                |SYSTEM      |2025-08-18 10:32:31.376 +0700|                                |                       |
COB050016_CO_9.4_33e24768-b3a6-45d8-8166-06d752262c2b|67b3aa6f0e884b59a73620ef815fa10b|officer_thai|2025-08-18 11:11:23.364 +0700|                                |                       |
COB050016_CO_6.1_b569b749-8211-4650-a783-f5029095d136|10d280cd7975451f8ca5c732d84a3c49|officer_thai|2025-08-18 11:06:52.749 +0700|                                |                       |
```

</div>

</div>

## **3. Check orphaned protocol for APP**

### Query:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cb413196-84b4-4eb4-9032-e6967c0a22d4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
WITH dossier_counter AS (
    SELECT businessdataid AS id,
        json_extract_path_text(objectvalue::json, 'value')::INTEGER AS current_cob_number
    FROM iwa_businessdata
    WHERE objecttype = 'ch.axonivy.fintech.standard.dossier.DossierIdGenerator$DossierIdSeed'
        AND businessdataid = 'APP'
),
orphaned_protocol AS (
    SELECT dossierid,
        externalreference,
        MIN(username) AS username,
        MAX("timestamp") AS timestamp
    FROM public.protocol
    WHERE dossierid LIKE 'APP%'
        AND substring(dossierid, 4, 6)::INTEGER > (SELECT current_cob_number FROM dossier_counter)
    GROUP BY dossierid, externalreference
)
SELECT orphaned_protocol.*,
    iwa_businessdata.businessdataid,
    iwa_businessdata.modifiedat
FROM orphaned_protocol
    LEFT JOIN iwa_businessdata
    ON orphaned_protocol.externalreference = iwa_businessdata.businessdataid;
```

</div>

</div>

## **4. Check orphaned protocol for WEB**

### Query:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="64fb923b-82aa-4a02-a2c2-3b0e24ba32e0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
WITH dossier_counter AS (
    SELECT businessdataid AS id,
        json_extract_path_text(objectvalue::json, 'value')::INTEGER AS current_cob_number
    FROM iwa_businessdata
    WHERE objecttype = 'ch.axonivy.fintech.standard.dossier.DossierIdGenerator$DossierIdSeed'
        AND businessdataid = 'WEB'
),
orphaned_protocol AS (
    SELECT dossierid,
        externalreference,
        MIN(username) AS username,
        MAX("timestamp") AS timestamp
    FROM public.protocol
    WHERE dossierid LIKE 'WEB%'
        AND substring(dossierid, 4, 6)::INTEGER > (SELECT current_cob_number FROM dossier_counter)
    GROUP BY dossierid, externalreference
)
SELECT orphaned_protocol.*,
    iwa_businessdata.businessdataid,
    iwa_businessdata.modifiedat
FROM orphaned_protocol
    LEFT JOIN iwa_businessdata
    ON orphaned_protocol.externalreference = iwa_businessdata.businessdataid;
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[SQL script to get SOB (APP & WEB) dossier progress]]
- [[Script identify fields which are incorrectly logged in the protocol]]
- [[SQL script to investigate dossier COB003059 on INT1]]
- [[SQL script to update retry tasks and check pending dossiers has an outdated credit card]]
- [[SQL script to find all documents from Fidentity]]

%% ai-graph-end %%