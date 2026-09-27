---
title: "Script | identify fields which are incorrectly logged in the protocol"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134182477/Script+identify+fields+which+are+incorrectly+logged+in+the+protocol
space: "GRAVITY"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2022-06-24
attachments: 0
tags:
  - confluence
  - programming
  - space/gravity
---

# Script | identify fields which are incorrectly logged in the protocol

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2022-06-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134182477/Script+identify+fields+which+are+incorrectly+logged+in+the+protocol)
> Relevance 0.731 · topic `programming`

These scripts are written from the requirement of the user story: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47134182477_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="FFAG-45280" macro-id="2d3c99b6-1e45-4286-a826-83f386b03bdb" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/FFAG-45280" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>FFAG-45280</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

***Script 1:*** Get dossier number where the field “TIN” or checkbox “TIN cannot be provided“ is logged by the system.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5c74bee9-06e6-47cb-938b-5ad62f7b8e9e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT distinct substring(pro_filtered.dossierid,0,10) dossier_id
        FROM (SELECT dossierid, protocolid, timestamp 
                FROM protocol pro 
                        WHERE pro.username ='SYSTEM' ----logged by system
                                AND pro.timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                AND substring(pro.dossierid,0,10) IN (WITH
                                                      businessdata AS (
                                                          SELECT cast(objectvalue as json) as dossier FROM iwa_businessdata WHERE objecttype='ch.axonivy.desk.individual.IndividualDossier')
                                                      , dossiers AS (
                                                        SELECT (dossier ->> 'cobId') AS cobId FROM businessdata
                                                                                                WHERE ((dossier -> 'stopOnboardingInformation' ->> 'stopOnboardingDate') IS NULL 
                                                                                                    AND (dossier -> 'complianceTasks' ->> 1) NOT LIKE '%"isNeedToStopOnboarding":true%') --- pending
                                                                                                    OR ((dossier -> 'progressInfo' ->> 'completionDate') IS NOT NULL)  --- completed
                                                        )
                                                      SELECT distinct substring(cobId,0,10) as dossierid FROM dossiers)
         ) pro_filtered, protocol_entry entry 
            WHERE ((entry.attributeidentifier LIKE '%sTinNotProvided' AND entry.value='true') OR entry.attributeidentifier LIKE '%taxIdentificationNumber')
                  AND entry.fk_protocol_identifier = pro_filtered.protocolid;
```

</div>

</div>

***Script 2:*** Get dossier number where the radio button “Are you a taxable person in the USA?” in the questionnaire is logged by the system.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8540a157-214b-4bc2-9baa-4278d22f4ed9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT distinct substring(pro_filtered.dossierid,0,10) as dossier_id 
    FROM ( SELECT dossierid, protocolid, timestamp, username 
                FROM protocol pro
                        WHERE pro.username ='SYSTEM' ----logged by system
                            AND pro.timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                            AND pro.dossierid IN ( WITH
                                                      businessdata AS (
                                                          SELECT cast(objectvalue as json) as dossier FROM iwa_businessdata WHERE objecttype='ch.axonivy.desk.individual.IndividualDossier')
                                                      , dossiers AS (
                                                        SELECT (dossier ->> 'cobId') AS cobId FROM businessdata
                                                                                                        WHERE ((dossier -> 'progressInfo' ->> 'completionDate') IS NOT NULL)  --- completed
                                                                                                                )
                                                      SELECT distinct substring(cobId,0,10) as dossierid FROM dossiers)
                                                      
         ) pro_filtered, protocol_entry entry 
            WHERE attributeidentifier LIKE '%isTaxableUsaPerson' 
                    AND entry.fk_protocol_identifier = pro_filtered.protocolid
                    AND (entry.value = '' or entry.value is null)
                    AND entry.previousvalue is not null
                    AND entry.previousvalue != '';
```

</div>

</div>

***Script 3***: Get dossier number where the field “Please provide the other reason your address did could not be verified” in personal information is logged by the system.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8dfd1a8e-1a30-4a43-bb21-90a11c75ac43" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT distinct substring(pro_filtered.dossierid,0,10) as dossier_id   
    FROM ( SELECT dossierid, protocolid, timestamp 
                FROM protocol pro
                        WHERE pro.username ='SYSTEM' ----logged by system
                            AND pro.timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                            AND substring(pro.dossierid,0,10) IN ( WITH
                                                      businessdata AS (
                                                          SELECT cast(objectvalue as json) as dossier FROM iwa_businessdata WHERE objecttype='ch.axonivy.desk.individual.IndividualDossier')
                                                      , dossiers AS (
                                                        SELECT (dossier ->> 'cobId') AS cobId FROM businessdata
                                                                                                        WHERE ((dossier -> 'stopOnboardingInformation' ->> 'stopOnboardingDate') IS NULL 
                                                                                                                AND (dossier -> 'complianceTasks' ->> 1) NOT LIKE '%"isNeedToStopOnboarding":true%') --- pending
                                                                                                                OR ((dossier -> 'progressInfo' ->> 'completionDate') IS NOT NULL)  --- completed
                                                                                                                )
                                                      SELECT distinct substring(cobId,0,10) as dossierid FROM dossiers)
         ) pro_filtered, protocol_entry entry 
            WHERE attributeidentifier LIKE '%address.nokOtherReason%'
                    AND entry.fk_protocol_identifier = pro_filtered.protocolid
                    AND EXISTS (SELECT dossierid FROM  protocol pr, protocol_entry en 
                                                WHERE pr.dossierid = pro_filtered.dossierid 
                                                    AND en.attributeidentifier LIKE '%address.nokReason'
                                                    AND en.fk_protocol_identifier = pr.protocolid
                                                    AND en.value ='OTHER'
                                                    AND pr.timestamp = (SELECT max(timestamp) 
                                                                                FROM protocol, protocol_entry
                                                                                    WHERE attributeidentifier = en.attributeidentifier
                                                                                            AND dossierid = pro_filtered.dossierid
                                                                                            AND fk_protocol_identifier = protocolid)    
                    );
```

</div>

</div>

***Script 4***: Get dossier number where the radio button “Does your current signature match the one on the uploaded ID?” in identification is set to “No“ and logged by KuBe.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e932803c-46f2-4c6d-834c-a54dfca21498" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT distinct substring(dossierid,0,10) as dossier_id 
    FROM (SELECT dossierid, attributeidentifier, value
            FROM (SELECT dossierid, protocolid, timestamp, username 
                    FROM protocol pro
                        WHERE pro.timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                            AND pro.dossierid IN ( WITH businessdata AS (SELECT cast(objectvalue as json) as dossier FROM iwa_businessdata WHERE objecttype='ch.axonivy.desk.individual.IndividualDossier')
                                                      , dossiers AS (
                                                        SELECT (dossier ->> 'cobId') AS cobId FROM businessdata
                                                                                                        WHERE ((dossier -> 'stopOnboardingInformation' ->> 'stopOnboardingDate') IS NULL 
                                                                                                                AND (dossier -> 'complianceTasks' ->> 1) NOT LIKE '%"isNeedToStopOnboarding":true%') --- pending
                                                                                                                OR ((dossier -> 'progressInfo' ->> 'completionDate') IS NOT NULL)  --- completed
                                                                                                                )
                                                      SELECT cobId as dossierid FROM dossiers)
                            AND pro.username != 'SYSTEM'                                              
         ) pro_filtered, protocol_entry entry WHERE attributeidentifier LIKE '%signatureMatchWithUploadedIdOption' AND entry.fk_protocol_identifier = pro_filtered.protocolid AND entry.value = 'NO' AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                    AND 
                         (NOT EXISTS (SELECT dossierid FROM protocol, protocol_entry WHERE attributeidentifier = entry.attributeidentifier
                                                                                            AND dossierid = pro_filtered.dossierid AND username = 'SYSTEM' AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                                                                            AND fk_protocol_identifier = protocolid and value = 'NO') --- not log by system yet
                         OR NOT EXISTS (SELECT dossierid FROM protocol, protocol_entry WHERE attributeidentifier LIKE '%signatureMatchWithUploadedIdOption' AND dossierid = pro_filtered.dossierid
                                                    AND value = 'YES' AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                                    AND timestamp > (SELECT max("timestamp") FROM protocol, protocol_entry WHERE attributeidentifier LIKE '%signatureMatchWithUploadedIdOption'
                                                                                        AND value = 'NO' AND dossierid = pro_filtered.dossierid AND username != 'SYSTEM' 
                                                                                        AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')))                                                                    
                         OR pro_filtered.dossierid IN (SELECT dossierid FROM protocol pro, protocol_entry WHERE attributeidentifier LIKE '%signatureMatchWithUploadedIdOption' AND username != 'SYSTEM' 
                                                                                                                AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s') AND value = 'NO' AND fk_protocol_identifier = protocolid
                                                                                                                AND timestamp = (SELECT max(timestamp) FROM protocol, protocol_entry WHERE attributeidentifier LIKE '%signatureMatchWithUploadedIdOption' AND username != 'SYSTEM' 
                                                                                                                                                                AND dossierid = pro.dossierid AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                                                                                                                                                AND fk_protocol_identifier = protocolid
                                                                                                                                                                AND EXISTS (SELECT max(timestamp) FROM protocol, protocol_entry WHERE attributeidentifier LIKE '%signatureMatchWithUploadedIdOption'
                                                                                                                                                                                                                                    AND username != 'SYSTEM' 
                                                                                                                                                                                                                                    AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                                                                                                                                                                                                                    AND value = 'NO'
                                                                                                                                                                                                                                    AND timestamp > pro.timestamp
                                                                                                                                                                                                                                    AND dossierid = pro.dossierid
                                                                                                                                                                                                                                    AND fk_protocol_identifier = protocolid)
                                                                                                               AND timestamp < (SELECT max(timestamp) FROM protocol, protocol_entry WHERE attributeidentifier LIKE '%signatureMatchWithUploadedIdOption' AND username = 'SYSTEM' 
                                                                                                                                                                                            AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                                                                                                                                                                            AND value = 'NO' AND timestamp > pro.timestamp
                                                                                                                                                                                            AND dossierid = pro.dossierid AND fk_protocol_identifier = protocolid)))                                                                                                                                                                                            
                         )) as pro -- excluse case system and kube save similar value and kube not show in protocol
                            WHERE (SELECT max(timestamp) FROM protocol, protocol_entry WHERE attributeidentifier = pro.attributeidentifier AND dossierid = pro.dossierid AND fk_protocol_identifier = protocolid    
                                                                                            AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')) = (SELECT max(timestamp) FROM protocol, protocol_entry WHERE attributeidentifier = pro.attributeidentifier
                                                                                                                                                                                                                            AND dossierid = pro.dossierid AND value = 'NO'
                                                                                                                                                                                                                            AND username != 'SYSTEM' 
                                                                                                                                                                                                                            AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                                                                                                                                                                                                            AND fk_protocol_identifier = protocolid)
                                AND (not exists (SELECT dossierid FROM protocol pr, protocol_entry en 
                                                        WHERE pr.dossierid = pro.dossierid
                                                            AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                                            AND en.attributeidentifier LIKE '%signatureMatchWithUploadedIdOption'
                                                            AND en.fk_protocol_identifier = pr.protocolid
                                                            AND pr.timestamp >= (SELECT max(timestamp) FROM protocol, protocol_entry
                                                                                                            WHERE attributeidentifier LIKE '%signatureMatchWithUploadedIdOption' AND username = 'SYSTEM' 
                                                                                                                AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s') AND value = 'NO' AND dossierid = pro.dossierid
                                                                                                                AND timestamp < (SELECT max(timestamp) FROM protocol, protocol_entry WHERE attributeidentifier  LIKE '%signatureMatchWithUploadedIdOption'
                                                                                                                                                            AND dossierid = pro.dossierid AND value = 'NO' AND username != 'SYSTEM' 
                                                                                                                                                            AND timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                                                                                                                                            AND fk_protocol_identifier = protocolid)
                                                                                                                AND fk_protocol_identifier = protocolid)    
                                                 GROUP BY dossierid HAVING count(username) > 1 AND count(distinct value) = 1))    --- exclude case kube log NO after system -> not show in protocol
```

</div>

</div>

***Script 5:*** Get dossier number where two KuBe’s were working on the same dossier.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9a603514-a0b7-4579-9e76-9a20fc25cd82" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT substring(pro.dossierid,0,10) as dossier_id FROM protocol pro
    WHERE pro.protocolid IN (SELECT protocolid FROM protocol pro, iwa_user us, iwa_userrole us_r, iwa_role rl  
                                                WHERE pro.username = us.name 
                                                        AND us.userid = us_r.userid 
                                                        AND us_r.roleid = rl.roleid 
                                                        AND rl.name = 'BankEmployee') 
        AND dossierid IN ( WITH
                            businessdata AS (
                            SELECT cast(objectvalue as json) as dossier FROM iwa_businessdata WHERE objecttype='ch.axonivy.desk.individual.IndividualDossier')
                            , dossiers AS (
                            SELECT (dossier ->> 'cobId') AS cobId FROM businessdata
                                    WHERE ((dossier -> 'stopOnboardingInformation' ->> 'stopOnboardingDate') IS NULL 
                                            AND (dossier -> 'complianceTasks' ->> 1) NOT LIKE '%"isNeedToStopOnboarding":true%') --- pending
                                            OR ((dossier -> 'progressInfo' ->> 'completionDate') IS NOT NULL)  --- completed
                            )
                            SELECT distinct substring(cobId,0,10) as dossierid FROM dossiers)
        AND pro.timestamp > to_date('11/02/2022', 'dd/mm/yyyy hh:m:s')                                           
            GROUP BY pro.dossierid 
                HAVING count(DISTINCT pro.username) > 1
                    ORDER BY pro.dossierid; 
```

</div>

</div>

***Script 6.1:*** Get dossier number where the officer is logged in the following fields: category of the foreign permit

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="39966fe0-3574-4ea1-9e57-a9a376af9588" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT distinct substring(dossierid,0,10)  as dossier_id 
    FROM protocol_entry entry 
        ,(SELECT dossierid, protocolid FROM protocol pro, iwa_user us, iwa_userrole us_r, iwa_role rl 
                                        WHERE pro.username = us.name 
                                            AND us.userid = us_r.userid 
                                            AND us_r.roleid = rl.roleid 
                                            AND rl.name = 'ComplianceOfficer'
                                            AND pro.timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                            AND substring(pro.dossierid,0,10) in (WITH
                                                        businessdata AS (SELECT cast(objectvalue as json) as dossier FROM iwa_businessdata
                                                                                                                        WHERE objecttype='ch.axonivy.desk.individual.IndividualDossier')
                                                        , dossiers AS (SELECT (dossier ->> 'cobId') AS cobId FROM businessdata
                                                                                                                WHERE ((dossier -> 'stopOnboardingInformation' ->> 'stopOnboardingDate') IS NULL
                                                                                                                AND (dossier -> 'complianceTasks' ->> 1) NOT LIKE '%"isNeedToStopOnboarding":true%')
                                                                                                                OR ((dossier -> 'progressInfo' ->> 'completionDate') IS NOT NULL)  ---completed
                                                        )
                                                        SELECT distinct cobId as dossierid FROM dossiers)) as officer_logged
            WHERE entry.attributeidentifier LIKE '%identificationTypeCategory%'
                    AND entry.fk_protocol_identifier = officer_logged.protocolid;
```

</div>

</div>

***Script 6.2:*** Get dossier number where the officer is logged in the following fields: the document status of the id document.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fbc97407-cd25-4b0e-ab99-49fa2bc7bb96" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT distinct substring(officer_logged.dossierid,0,10) as dossier_id
    FROM (SELECT fk_protocol_identifier FROM protocol_entry en, (SELECT distinct substring(attributeidentifier,'\[(.*)\]') as keys FROM protocol_entry entry, protocol pro 
                                                                                                                                            WHERE value like '%IDENTIFICATION%' 
                                                                                                                                                    AND entry.fk_protocol_identifier = pro.protocolid) as protocol_keys
                                                                WHERE en.attributeidentifier like '%' || protocol_keys.keys || '%' 
                                                                    AND  en.attributeidentifier like '%documentStatus') as id_document_status
                                                , (SELECT pro.protocolid, dossierid, username FROM protocol pro, iwa_user us, iwa_userrole us_r, iwa_role rl 
                                                                                    WHERE pro.username = us.name
                                                                                        AND us.userid = us_r.userid 
                                                                                        AND us_r.roleid = rl.roleid 
                                                                                        AND rl.name = 'ComplianceOfficer'
                                                                                        AND pro.timestamp > to_date('11/02/2022 12:00:00', 'dd/mm/yyyy hh:m:s')
                                                    ) officer_logged
                                                WHERE id_document_status.fk_protocol_identifier = officer_logged.protocolid
                                                        AND length(officer_logged.dossierid) < 10
                                                        AND officer_logged.dossierid in (WITH
                                                                                          businessdata AS (SELECT cast(objectvalue as json) as dossier FROM iwa_businessdata
                                                                                                                                                            WHERE objecttype='ch.axonivy.desk.individual.IndividualDossier')
                                                                                          , dossiers AS (SELECT (dossier ->> 'cobId') AS cobId FROM businessdata
                                                                                                                                                    WHERE ((dossier -> 'stopOnboardingInformation' ->> 'stopOnboardingDate') IS NULL
                                                                                                                                                                AND (dossier -> 'complianceTasks' ->> 1) NOT LIKE '%"isNeedToStopOnboarding":true%')
                                                                                                                                                            OR ((dossier -> 'progressInfo' ->> 'completionDate') IS NOT NULL)  ---completed
                                                                                          )
                                                                                          SELECT distinct cobId as dossierid FROM dossiers);
```

</div>

</div>

***Script 7***: Get dossier number where a finform user (BE or BP) has updated a dossier.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f99d07ee-735b-4943-93ab-c4fb7b1a1944" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT distinct substring(pro.dossierid,0,10) dossier_id 
                FROM protocol pro 
                        WHERE pro.username IN ('samir.isis.BE@finform.ch',

                                            'fabian.hutmacher.BE@finform.ch',

                                            'minh.nguyen.BE@finform.ch',

                                            'reto.luethi.BE@finform.ch',

                                            'sara.espinoza.BE@finform.ch',

                                            'selin.yavuz.BE@finform.ch',
                                            
                                            'michele.rigert.BE@finform.ch',
                                            
                                            'stefani.ahmetspahic.BE@finform.ch',
                                            
                                            'chris.reusser.BE@finform.ch',
                                            
                                            'chris.reusser.BP@finform.ch',
                                            
                                            'flaviano.faiazza.BE@finform.ch',
                                            
                                            'flaviano.faiazza.BP@finform.ch')
                        
                                AND substring(pro.dossierid,0,10) IN ( WITH
                                                      businessdata AS (
                                                          SELECT cast(objectvalue as json) as dossier FROM iwa_businessdata WHERE objecttype='ch.axonivy.desk.individual.IndividualDossier')
                                                      , dossiers AS (
                                                        SELECT (dossier ->> 'cobId') AS cobId FROM businessdata
                                                                                                        WHERE ((dossier -> 'stopOnboardingInformation' ->> 'stopOnboardingDate') IS NULL 
                                                                                                                AND (dossier -> 'complianceTasks' ->> 1) NOT LIKE '%"isNeedToStopOnboarding":true%') --- pending
                                                                                                                OR ((dossier -> 'progressInfo' ->> 'completionDate') IS NOT NULL)  --- completed
                                                                                                                )
                                                      SELECT distinct substring(cobId,0,10) as dossierid FROM dossiers);
```

</div>

</div>
