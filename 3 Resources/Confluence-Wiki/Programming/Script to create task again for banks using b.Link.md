---
title: "Script to create task again for banks using b.Link"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/23200718451/Script+to+create+task+again+for+banks+using+b.Link
space: "NEXT"
topic: programming
relevance: 0.837
depth: 3
updated: 2020-10-29
attachments: 9
tags:
  - confluence
  - programming
  - space/next
---

# Script to create task again for banks using b.Link

> [!info] Imported from Confluence
> Space **NEXT** · updated 2020-10-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/23200718451/Script+to+create+task+again+for+banks+using+b.Link)
> Relevance 0.837 · topic `programming`

1.  Run the script below in **luz_key_value_store database** to get all tenants which has bLlink token

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bb68b772-5c6a-4bfb-8313-39600588a8b3" macro-name="code" style="border-width: 1px;">

    <div class="codeHeader panelHeader pdl hide-border-bottom">

    <span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

    </div>

    <div class="codeContent panelContent pdl hide-toolbar">

    ``` syntaxhighlighter-pre
    DO
    $$
    DECLARE
      _schema TEXT;
       numberOfCorAPIkey int DEFAULT 0;

    previous_step_output_line TEXT;
    BEGIN
        FOR _schema IN SELECT DISTINCT ON (nspname) quote_ident(nspname)
             FROM pg_catalog.pg_namespace n
                            JOIN pg_catalog.pg_class c ON n.oid = c.relnamespace
                            JOIN pg_catalog.pg_attribute a ON a.attrelid = c.oid
                        WHERE nspname LIKE 's_%' AND c.relname = 'key_value_store' 
                   -- IF you want to check specific tenants, please un-comment AND-condition below and replace the tenants as you wish
                   -- AND nspname IN ('s_f433fd2e_dd67_40fd_b98c_3fe9c9e1a621', 's_f433fd2e_dd67_40fd_b98c_3fe9c9e1a622')
            ORDER BY nspname
        LOOP
            EXECUTE 'set schema ''' || _schema || '''';                                                                     
                SELECT COUNT(*) into numberOfCorAPIkey FROM key_value_store WHERE kv_key like '%ch.klara.bank.corapi.token.%';
                IF numberOfCorAPIkey > 0
                THEN
                    RAISE NOTICE '%', _schema;
                    END IF;
        END LOOP;
    END;
    $$
    ```

    </div>

    </div>

    After running the script, please copy the messages (like the picture below) into the second script for **previous_step_output_line** variable  
    

![[23200718451-image2020-10-19_18-40-30.png]]



2.  Run the script below in **luz_finance database** to get all tenants that have Credit Suisse accounts and are connected to Blink

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f088af9d-bdbe-4717-8457-473d9efe138e" macro-name="code" style="border-width: 1px;">

    <div class="codeHeader panelHeader pdl hide-border-bottom">

    <span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

    </div>

    <div class="codeContent panelContent pdl hide-toolbar">

    ``` syntaxhighlighter-pre
    DO
    $$
    DECLARE
    _schema TEXT;
    companyBankAccount record;
    bankClearingNumbers VARCHAR[];
    bankShortName VARCHAR(25) := 'CS';

    schemaText TEXT;
    tenant_info TEXT[];

    previous_step_output_line TEXT;

    previous_step_output TEXT default '
    NOTICE:  s_28093111_eb6d_4b58_b5d8_c870fd2062f7
    NOTICE:  s_388c822c_7860_41ae_94ac_330684bb63e0
    NOTICE:  s_3a8caf92_e933_4d2d_97c1_4616c1a478e0
    NOTICE:  s_9375914b_65e6_48e9_9709_adb331fbd756
    NOTICE:  s_b0026385_b9db_44b0_8d4f_63dec9df0842
    NOTICE:  s_cebc9360_b364_4c50_b877_c47efd704237
    NOTICE:  s_d5593dd6_3586_47c1_9986_9cd64bed0f11
    NOTICE:  s_e2446ac5_14d4_4235_b567_9f5576c124b7
    '
    ;

    BEGIN

    SELECT ARRAY_AGG(distinct bank_clearing_number) INTO bankClearingNumbers FROM public.swiss_bank WHERE short_name in ('CSG AG', 'CS AG', 'CS ex Clariden', 'CS (Schweiz) AG');

    FOR _schema IN SELECT DISTINCT ON (nspname) quote_ident(nspname)
    FROM pg_catalog.pg_class c LEFT JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
    WHERE nspname !~~ 'pg_%'
    AND nspname <> 'information_schema'
    AND nspname <> 'public'
    AND c.relname = 'company_bank_account' 
    ORDER BY nspname

    LOOP
    EXECUTE 'set schema ''' || _schema ||'''';

    FOR companyBankAccount IN (
    SELECT DISTINCT ON (company_id) company_id, iban_number, CAST(substring(iban_number, 6, 4) AS INT) AS clearing_number
    FROM company_bank_account
    WHERE iban_number LIKE 'CH%'
    AND ( 
    (substring(iban_number, 7, 3) = ANY(bankClearingNumbers) AND substring(iban_number, 5, 2) = '00')
    OR
    (substring(iban_number, 6, 4) = ANY(bankClearingNumbers) AND substring(iban_number, 5, 1) = '0')
    )
    GROUP BY company_id, iban_number)
    LOOP
    -- INSERT INTO company_bank VALUES (_schema, companyBankAccount.company_id, companyBankAccount.iban_number, bankShortName, companyBankAccount.clearing_number);
    FOR previous_step_output_line IN SELECT regexp_split_to_table(previous_step_output, '\n')
    LOOP

    schemaText = TRIM(leading 'NOTICE: ' from previous_step_output_line);
    --RAISE notice 'list schema text: %', schemaText;
    IF schemaText = _schema
    THEN
    RAISE NOTICE '%;%;%;%;%', _schema, companyBankAccount.company_id, companyBankAccount.iban_number, bankShortName, companyBankAccount.clearing_number;
    END IF;
    END LOOP;
    END LOOP;

    END LOOP;

    END;
    $$
    ```

    </div>

    </div>

3.  After running the second script, we have a list of tenants in the message that need to be created task again  
    

![[23200718451-image2020-10-19_18-42-21.png]]



4.    Create a file with **\*.csv** format -\> Using **text editor** to open the file and copy the result above into the file and remove all **"NOTICE:  "  
    

![[23200718451-image2020-10-19_18-43-42.png]]

** 

5.  Using the file just created and accessing admin page → bank account setting → upload a file → click button "Create onboarding task" → see the result  
    

![[23200718451-image2020-10-19_18-44-53.png]]

 

![[23200718451-image2020-10-19_18-45-21.png]]

![[23200718451-image2020-10-19_18-50-36.png]]



6.   Running this script in **luz_tenant database**. Using the result in **Step 3** for below script to get the company name, tenant id, user name for CS-connected tenants

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dcbf2a16-3fdb-420f-9b0e-9ce3ee3be0f3" macro-name="code" style="border-width: 1px;">

    <div class="codeHeader panelHeader pdl hide-border-bottom">

    <span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

    </div>

    <div class="codeContent panelContent pdl hide-toolbar">

    ``` syntaxhighlighter-pre
    DO
    $$
    DECLARE
    _schema TEXT;
    schemaText TEXT;
    tenant_id TEXT;
    company_name TEXT;
    user_name TEXT;
    previous_step_output_line TEXT;
     
    previous_step_output TEXT default '
    NOTICE:  s_28093111_eb6d_4b58_b5d8_c870fd2062f7;4;CH5204835012345671000;CS;4835
    NOTICE:  s_388c822c_7860_41ae_94ac_330684bb63e0;1;CH3204835115938372002;CS;4835
    NOTICE:  s_3a8caf92_e933_4d2d_97c1_4616c1a478e0;1;CH3204835115938372002;CS;4835
    NOTICE:  s_9375914b_65e6_48e9_9709_adb331fbd756;1;CH3204835115938372002;CS;4835
    NOTICE:  s_b0026385_b9db_44b0_8d4f_63dec9df0842;1;CH5804835039343914177;CS;4835
    NOTICE:  s_cebc9360_b364_4c50_b877_c47efd704237;1;CH5204835012345671000;CS;4835
    NOTICE:  s_d5593dd6_3586_47c1_9986_9cd64bed0f11;1;CH3204835115938372002;CS;4835
    NOTICE:  s_e2446ac5_14d4_4235_b567_9f5576c124b7;1;CH5204835012345671000;CS;4835
    '
    ;
     
    BEGIN
    FOR previous_step_output_line IN SELECT regexp_split_to_table(previous_step_output, '\n')
    LOOP

    SELECT INTO tenant_id SUBSTRING(previous_step_output_line from 12 for 36);
    SELECT INTO tenant_id REPLACE(tenant_id, '_', '-' );

    SELECT companyname into company_name FROM public.companyinfo WHERE tenant_id = tenantid;
    SELECT username into user_name FROM public.tenant WHERE tenant_id = tenantid;
     
    RAISE NOTICE 'tenantId: %, company name: %, username: %', tenant_id, company_name, user_name;
     
    END LOOP;
    END;
    $$
    ```

    </div>

    </div>

    The result will be listed like this:

    

![[23200718451-image2020-10-19_19-43-1.png]]



7.  Running the script below in **luz_person database** by using the result in step 7 to get telephone number of each company  

      

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2d08b13b-d59f-4c2d-b550-8a1d1e058504" macro-name="code" style="border-width: 1px;">

    <div class="codeHeader panelHeader pdl hide-border-bottom">

    <span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

    </div>

    <div class="codeContent panelContent pdl hide-toolbar">

    ``` syntaxhighlighter-pre
    DO
    $$
    DECLARE
    _schema TEXT;

    previous_step_output_line TEXT;
    company_info TEXT[];
    company_schema TEXT;
    company_name TEXT;
    companyId bigint;
    phoneId bigint;
    company_phone TEXT;
    company_with_phone TEXT;

    previous_step_output TEXT default '
    NOTICE: tenantId: 28093111-eb6d-4b58-b5d8-c870fd2062f7, company name: All BYs Are Sealed, username: hcmc-next@axonactive.com
    NOTICE: tenantId: 388c822c-7860-41ae-94ac-330684bb63e0, company name: Next Company, username: chris.sutter@axonivy.com
    NOTICE: tenantId: 3a8caf92-e933-4d2d-97c1-4616c1a478e0, company name: BBB Immobilien AG, username: jamoser42@gmail.com
    NOTICE: tenantId: 9375914b-65e6-48e9-9709-adb331fbd756, company name: AAA Holding SA, username: jamoser@yahoo.com
    NOTICE: tenantId: b0026385-b9db-44b0-8d4f-63dec9df0842, company name: Hieu Group, username: hcmc-next@axonactive.com
    NOTICE: tenantId: cebc9360-b364-4c50-b877-c47efd704237, company name: ACD GmbH, username: hcmc-next@axonactive.com
    NOTICE: tenantId: d5593dd6-3586-47c1-9986-9cd64bed0f11, company name: Maxmotos GmbH, username: klara_test_01@axonivy.io
    NOTICE: tenantId: e2446ac5-14d4-4235-b567-9f5576c124b7, company name: Process AG, username: hcmc-next@axonactive.com
    '
    ;

    BEGIN

    FOR previous_step_output_line IN SELECT regexp_split_to_table(previous_step_output, '\n')
    LOOP
    company_info = (SELECT regexp_split_to_array(previous_step_output_line, ','));
    company_schema = TRIM((SELECT regexp_split_to_array(company_info[1], ':'))[3]);
    company_schema = REPLACE(company_schema , '-', '_'); 
    company_name = TRIM((SELECT regexp_split_to_array(company_info[2], ':'))[2]);

    FOR _schema IN SELECT DISTINCT ON (nspname) quote_ident(nspname)
    FROM pg_catalog.pg_class c LEFT JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
    WHERE nspname !~~ 'pg_%'
    AND nspname <> 'information_schema'
    AND nspname <> 'public'
    AND nspname LIKE CONCAT('s_', company_schema)
    AND c.relname = 'company'
    ORDER BY nspname

    LOOP
    EXECUTE 'set schema ''' || _schema ||'''';

    SELECT id into companyId FROM company WHERE company_name = name;
    SELECT phone_id into phoneId FROM company_phone WHERE company_id = companyId;
    SELECT phone_number into company_phone FROM phone WHERE phoneId = id;

    company_with_phone = CONCAT(previous_step_output_line, ', telephone: ', company_phone);
    RAISE NOTICE '%' , company_with_phone;
    END LOOP;
    END LOOP;
    END;
    $$
    ```

    </div>

    </div>

    The result will be listed like this:  
    

![[23200718451-image2020-10-21_13-25-49.png]]
