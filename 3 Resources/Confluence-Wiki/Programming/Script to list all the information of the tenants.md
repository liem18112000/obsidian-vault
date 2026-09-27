---
ai_hash: 9708c35b0d15b6db
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.792
source: https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/47441052081/Script+to+list+all+the+information+of+the+tenants
space: NEXT
status: reference
tags:
- confluence
- programming
- space/next
title: Script to list all the information of the tenants
topic: programming
type: source
updated: 2023-08-16
---

# Script to list all the information of the tenants

> [!info] Imported from Confluence
> Space **NEXT** · updated 2023-08-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/47441052081/Script+to+list+all+the+information+of+the+tenants)
> Relevance 0.792 · topic `programming`

This script must be run in luz_person and consume a list of tenant as an input

1/ List of input tenants as text

<div id="expander-985939373" class="expand-container conf-macro output-block" hasbody="true" macro-id="38a013d1-b751-40be-b99b-1017bd691d0e" macro-name="expand">

<div id="expander-control-985939373" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Get tenant info without phoneNumber</span>

</div>

<div id="expander-content-985939373" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="35c5c502-532e-4e5c-b572-4ba75cfe7bf8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
CREATE OR REPLACE FUNCTION get_all_company_info_by_tenant() RETURNS TABLE (company_tenant_info varchar) AS
$BODY$
    DECLARE
       _schema  VARCHAR(50);
       tenantId  VARCHAR(50);
       company_language  VARCHAR(10);
       company_email  VARCHAR(100);
       companyTenantInfo record;
       allSchemas text = '';
    begin
         -- create temporary table and insert a header
        CREATE TEMPORARY TABLE company_info (company_tenant_info VARCHAR(300)) ON COMMIT DROP;
       INSERT INTO company_info VALUES ('tenantid' || ';' ||
                                                       'companyName'|| ';' ||
                                                       'language'|| ';'|| 
                                                       'emailAddress');
                                                      
        -- loop through all tenants
       FOR _schema IN select * from unnest(string_to_array(allSchemas, ',')) as t(object_id)  LOOP
        
        EXECUTE 'set schema ''' || _schema || '''';
       
       
       -- extract tenant information: name, language, email address      
        execute format('select c.name,  c.language,e.email_address from company c , company_email ce, email e where c.id = ce.company_id and e.id = ce.email_id') into companyTenantInfo;
        if (companyTenantInfo.language IS NOT null) and (companyTenantInfo.language <> '') then
            company_language := companyTenantInfo.language;
        else
            company_language := 'undefined';
        end if;
       
       if (companyTenantInfo.email_address IS NOT null) and (companyTenantInfo.email_address <> '') then
            company_email := companyTenantInfo.email_address;
        else
            company_email := 'undefined';
        end if;
        
       -- convert schema to tenantId
        tenantId := _schema;
        execute format('SELECT REPLACE(''%s'', ''%s'', ''%s'');',tenantId,'s_','') into tenantId;
        execute format('SELECT REPLACE(''%s'', ''%s'', ''%s'');',tenantId,'_','-') into tenantId;
       
       
         INSERT INTO company_info VALUES (tenantId || ';' ||
                                                       companyTenantInfo.name|| ';' ||
                                                       company_language|| ';'|| 
                                                       company_email);
        END LOOP;
 
        RETURN  query SELECT * FROM company_info;
        RETURN;
    END;
$BODY$
LANGUAGE plpgsql;
SELECT * FROM get_all_company_info_by_tenant();

--COPY (SELECT * FROM get_all_company_info_by_tenant()) TO 'D:\test_data\company_info_elm.csv';
```

</div>

</div>

</div>

</div>

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="e767464a-10ea-41ba-92a5-dbb92b4044b2" macro-name="view-file"><a href="../_attachments/47441052081-Step2_get_tenant_information.sql" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47441052081/Step2_get_tenant_information.sql?version=1&amp;modificationDate=1690164797380&amp;cacheVersion=1&amp;api=v2" data-mime-type="binary/octet-stream" data-has-thumbnail="true">

![[47441052081-Step2_get_tenant_information.sql]]

</a></span>

2/ List of input tenants as array

<div id="expander-2052487671" class="expand-container conf-macro output-block" hasbody="true" macro-id="f5e13ebb-4b7b-48b9-a1f7-6f058bcac7d9" macro-name="expand">

<div id="expander-control-2052487671" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Get tenant info with phoneNumber</span>

</div>

<div id="expander-content-2052487671" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dad073ae-1bb4-4d7c-8c59-ef97ad4c78d4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre

/*--------------------- Please read the note before executing the script! ---------------------
 * This script will list out company and contact information of it.
 * 
 * NOTE: 
 *      - Please execute on the database luzperson!
 *      - Please replace entries in the array _listOfSchema by your wishes
 *        In this case: It is the result of the second script above.
  ---------------------------------------------------------------------------------------------*/

CREATE OR REPLACE FUNCTION getCompanyInfos() RETURNS TABLE (tenant_id varchar, company_name varchar, company_language varchar, email varchar, phone varchar) AS
$BODY$
DECLARE

    --NOTE: Please remove the latest ',' of the last entry in this array if it is existing!!!
    _listOfSchema varchar[] := '{s_e2446ac5_14d4_4235_b567_9f5576c124b7,s_ff81385e_6dbe_45ef_afca_40f534230e1b,
                                s_089d935e_dbce_4f26_999a_1129dd582726,s_089d935e_dbce_4f26_999a_1129dd582726,
                                s_1fdb49d0_8516_47b7_935f_6d7b2cb24552,s_388c822c_7860_41ae_94ac_330684bb63e0,
                                s_3791d53b_e875_42ca_8ac7_77e726158f13,s_3791d53b_e875_42ca_8ac7_77e726158f13,
                                s_38e5971c_863d_41ec_88ad_aec3813e3905,s_54457738_5138_4e8a_ae80_be460cba6123}';
    _schema  varchar;
    _tenantId  varchar;
BEGIN
    -- create temporary table and insert a header
    CREATE TEMPORARY TABLE company_info (tenant_id varchar, company_name varchar, company_language varchar, email varchar, phone varchar) ON COMMIT DROP;
                                                      
    FOREACH _schema IN ARRAY _listOfSchema
    LOOP
        EXECUTE 'set schema ''' || _schema || '''';
        
        RAISE NOTICE 'Curremt schema: %', _schema;

        -- convert schema to tenantId
        _tenantId := _schema;
        SELECT  REPLACE(_tenantId, 's_', '') into _tenantId;
        SELECT  REPLACE(_tenantId, '_', '-') into _tenantId;     
       
        IF NOT EXISTS (SELECT 1 FROM company_info ci WHERE ci.tenant_id = _tenantId) 
        THEN 
            INSERT INTO company_info(tenant_id, company_name, company_language, email, phone) 
                    SELECT _tenantId, c.name, c.language, e.email_address, p.phone_number 
                        FROM company c, company_email ce, email e, company_phone cp, phone p 
                        WHERE c.id = ce.company_id AND e.id = ce.email_id AND c.id = cp.company_id AND p.id = cp.phone_id
                        ORDER BY c.id ASC 
                        LIMIT 1;
        END IF;

    END LOOP;
 
    RETURN  query SELECT * FROM company_info;
    RETURN;
   
END;
$BODY$
LANGUAGE plpgsql;


-- NOTE: Uncomment the line below if you want to printout the result to a file. Please change the directory below to your directory!!!
--COPY (SELECT * FROM getCompanyInfos()) To 'C:\company_info.csv' With CSV DELIMITER ';';

-- NOTE: Uncomment the line below if you want to printout the result in this screen
-- SELECT * FROM getCompanyInfos(); 

-- Please run this command to delete the function after you finish
-- DROP FUNCTION getCompanyInfos();
```

</div>

</div>

</div>

</div>

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="e4d06430-6568-490f-b807-5113795a8319" macro-name="view-file"><a href="../_attachments/47441052081-Step-03_Get_tenant_information.sql" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47441052081/Step-03_Get_tenant_information.sql?version=3&amp;modificationDate=1692162284093&amp;cacheVersion=1&amp;api=v2" data-mime-type="binary/octet-stream" data-has-thumbnail="true">

![[47441052081-Step-03_Get_tenant_information.sql]]

</a></span>

%% ai-graph-start %%

**Related notes:**
- [[SQL Script execution]]
- [[14. Create companies by tenant id]]
- [[15. Update companies by tenant id]]
- [[LUZ-102045 Implement physical delete for COMPANY tenant Part 2 (Postgres cont)]]
- [[Delete company - Old way]]

%% ai-graph-end %%