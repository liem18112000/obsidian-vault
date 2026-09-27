---
title: "Deactivate the Valiant/Finnova interface"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/47466315972/Deactivate+the+Valiant+Finnova+interface
space: "NEXT"
topic: programming
relevance: 0.786
depth: 3
updated: 2023-08-22
attachments: 5
tags:
  - confluence
  - programming
  - space/next
---

# Deactivate the Valiant/Finnova interface

> [!info] Imported from Confluence
> Space **NEXT** · updated 2023-08-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/47466315972/Deactivate+the+Valiant+Finnova+interface)
> Relevance 0.786 · topic `programming`

To deactivate the Valiant Finnova connection ( <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47466315972_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-104286" macro-id="810fd094-00ee-4ee7-82dc-a1342f8296af" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-104286" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-104286</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> ), there are 2 things need to be done by SQL script.

1.  Delete all available Valiant Finnova refresh tokens

2.  Copy the lastSyncDate from Valiant Finnova to b.Link if there is no lastSyncDate in Valiant b.Link yet

    - The first script will get all Valiant Finnova lastSyncDate (db luz_finnova)

    - The second script will insert/update lastSyncDate of Valiant Finnova (result of the first script) into Valiant b.Link (db key_value_store)  

Here are the scripts. They have to be executed one by one in the order as below. It is because the result of the pervious step will be input value for the next one.

**Important**: Please read the comment carefully before execute each SQL script!

1.  Delete all available Valiant Finnova refresh tokens and get lastSyncDate of Valiant ibans (database luzfinnova).  
    This script has to executed on database luzfinnova. It will do 2 things:  
    - DELETE all Valiant refresh tokens from Finnova.  
    - RETURN list of schemas which have Valiant connection and their lastSyncDates of Valiant ibans. This result will be used in the next script.  

<div id="expander-653927526" class="expand-container conf-macro output-block" hasbody="true" macro-id="88c8d02b-e2c0-4f06-9aaa-6bcf2f2b3e8c" macro-name="expand">

<div id="expander-control-653927526" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Delete all available Valiant Finnova refresh tokens and get lastSyncDate of Valiant ibans</span>

</div>

<div id="expander-content-653927526" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="47687ba3-bce3-497e-b0d2-57047798b3cb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/*--------------------- Please read the note before executing the script! ---------------------
 * This script will do for 2 things:
 *  - DELETE all Valiant refresh tokens from Finnova.
 *  - RETURN list of schemas which have Valiant connection and their lastSyncDates of Valiant ibans.
 * 
 * BECAREFULL!!!
 *     The flag _dryRunMode = 1: There is nothing DELETED after the script executed.
 *     The flag _dryRunMode = 0: The Valiant refresh tokens will be DELETED completely.  
 * 
 * Result/Output: List of schemas (tenants) which have Valiant connection deleted and lastSyncDate of each Valiant ibans.
 *
 * NOTE: please execute on the database luzfinnova!
  ---------------------------------------------------------------------------------------------*/
CREATE OR REPLACE FUNCTION deleteValiantRefreshTokensAndGetLastSyncDateOfValiantAccounts() RETURNS text AS
$BODY$
DECLARE
    _dryRunMode INT DEFAULT 1; -- CHANGE IT TO 0 to really delete Valiant refresh tokens.

    _listOfSchemaPrefixes VARCHAR[] := '{s_a%, s_b%, s_c%, s_d%, s_e%, s_f%, s_0%, s_1%, s_2%, s_3%, s_4%, s_5%, s_6%, s_7%, s_8%, s_9%}';
    _schemaPrefix  VARCHAR;
    _schema  VARCHAR;

    _syncInfo RECORD;
    _schemaValiantIbans TEXT;

BEGIN
    FOREACH _schemaPrefix IN ARRAY _listOfSchemaPrefixes 
    LOOP
        RAISE NOTICE 'Current schemaPrefix: %', _schemaPrefix;
        
        FOR _schema IN SELECT quote_ident(nspname)
            FROM pg_catalog.pg_class c LEFT JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
            WHERE nspname LIKE _schemaPrefix
               AND c.relname = 'bank_oauth_refreshtoken' 
               AND pg_relation_size(c.oid) > 0
        LOOP
            EXECUTE 'set schema ''' || _schema || '''';
        
            -- NOTE: Comment out this line if you want to see which schema executed at runtime
            -- RAISE NOTICE 'Start checking for schema: % ', _schema;
        
            IF exists (SELECT 1 FROM bank_oauth_refreshtoken WHERE refreshtoken notnull AND bankname LIKE 'valiant%')
            THEN
                FOR _syncInfo IN SELECT account_number, sync_date FROM bank_synchronization_info bsi WHERE account_number LIKE 'CH%' AND substring(account_number, 6, 4) = '6300'
                LOOP
                    -- NOTE: Comment out this line if you want to see the result at runtime
                    -- RAISE NOTICE 'Schema - iban - sync_date: % - % - %', _schema, _syncInfo.account_number, _syncInfo.sync_date;
                                        
                    SELECT CONCAT(_schema, ';', _syncInfo.account_number, ';', DATE(_syncInfo.sync_date), ',', _schemaValiantIbans) INTO _schemaValiantIbans;
                END LOOP;
               
               -- Delete Valiant refreshtoken from table bank_oauth_refreshtoken
               IF (_dryRunMode = 0) THEN 
                    DELETE FROM bank_oauth_refreshtoken WHERE refreshtoken notnull AND bankname LIKE 'valiant%';
               END IF;
            END IF;
        END LOOP;
    END LOOP;

    RETURN _schemaValiantIbans;
END;
$BODY$
LANGUAGE plpgsql;

-- NOTE: Uncomment the line below if you want to printout the result to a file. Please change the directory below to your directory!!!
--COPY (SELECT * FROM deleteValiantRefreshTokensAndGetLastSyncDateOfValiantAccounts()) To 'C:\schema_valiant_iban.csv' With CSV DELIMITER ';';

-- NOTE: Uncomment the line below if you want to printout the result in this screen
-- SELECT * FROM deleteValiantRefreshTokensAndGetLastSyncDateOfValiantAccounts(); 

-- Please run this command to delete the function after you finish
-- DROP FUNCTION deleteValiantRefreshTokensAndGetLastSyncDateOfValiantAccounts();
```

</div>

</div>

</div>

</div>

Example result:


![[47466315972-1b28706e-d86c-4cf5-9cf4-316ad534be23.png]]



2.  Migrate last sync date of Valiant accounts from Finnova connection to bLink connection (db key_value_store)  
    This script has to executed on database luzkeyvaluestore. It will insert the lastSyncDate from Valiant Finnova to Valiant b.Link if there was no lastSyncDate in Valiant b.Link available yet.  
    **NOTE**: Please replace the entries of the array **\_listOfLastSyncDatesFromFinnovaValiant** in this script by result of the first script above!  

<div id="expander-1010406771" class="expand-container conf-macro output-block" hasbody="true" macro-id="c7dabd5f-2e85-4620-99cf-1626d05b0804" macro-name="expand">

<div id="expander-control-1010406771" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Update/Insert lastSyncDate of Valiant ibans from Finnova connection to bLink connection</span>

</div>

<div id="expander-content-1010406771" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c99b69aa-6653-4d3c-a0d3-b3d585900aed" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/*--------------------- Please read the note before executing the script! ---------------------
* This script will INSERT the lastSyncDate from Valiant Finnova to Valiant b.Link if there was no lastSyncDate in Valiant b.Link available yet.
 * 
 * BECAREFULL!!!
 *     The flag _dryRunMode = 1: There is nothing INSERTED after the script executed.
 *     The flag _dryRunMode = 0: The last sync date of the Valaint iban from Finnova will be inserted into db for bLink. 
 * 
 * NOTE: 
 *     - Please execute on the database luzkeyvaluestore!
 *     - Please replace entries (schema and Valiant iban) in the array _listOfLastSyncDatesFromFinnovaValiant by your wishes.
 *     - Please run the last command to delete the function after you finish
  ---------------------------------------------------------------------------------------------*/
CREATE OR REPLACE FUNCTION insertLastSyncDatesFromValiantFinnovaToValiantBLink() RETURNS TABLE (schema_id VARCHAR, iban VARCHAR, last_sync_date VARCHAR) AS
$BODY$
DECLARE
    -- CHANGE IT TO 0 to really insert last sync date of Valiant iban from Finnova to db for bLink.
     _dryRunMode INT DEFAULT 1; 
     
    --NOTE: Please remove the latest ',' of the last entry in this array if it is existing!!!
    _listOfLastSyncDatesFromFinnovaValiant VARCHAR[] := '{s_54457738_5138_4e8a_ae80_be460cba6123;CH5206300016951553510;2022-02-25,
                                                        s_38e5971c_863d_41ec_88ad_aec3813e3905;CH5206300016951553510;2022-02-25,
                                                        s_3791d53b_e875_42ca_8ac7_77e726158f13;CH8506300430840114552;2022-02-25,
                                                        s_3791d53b_e875_42ca_8ac7_77e726158f13;CH5206300016951553510;2022-02-25,
                                                        s_388c822c_7860_41ae_94ac_330684bb63e0;CH8506300430840114552;2022-02-25,
                                                        s_1fdb49d0_8516_47b7_935f_6d7b2cb24552;CH8506300430840114552;2022-02-25,
                                                        s_089d935e_dbce_4f26_999a_1129dd582726;CH5206300016951553510;2022-02-25,
                                                        s_089d935e_dbce_4f26_999a_1129dd582726;CH8506300430840114552;2022-02-25,
                                                        s_ff81385e_6dbe_45ef_afca_40f534230e1b;CH7406300384375124504;2022-02-25,
                                                        s_e2446ac5_14d4_4235_b567_9f5576c124b7;CH5206300016951553510;2022-02-25}'; 
    _lastSyncDateFromFinnovaValiant VARCHAR;
    _lastSyncDateOfIban VARCHAR[];

    _schema  VARCHAR;
    _valiantIban VARCHAR;
    _lastSyncDate VARCHAR;
   
    _bLinkLastSyncDateKeyPrefix VARCHAR DEFAULT 'ch.klara.bank.corapi.synch.date.iban.';
    _bLinkLastSyncDateKey VARCHAR;
   
BEGIN
    -- create temporary table and insert a header
    CREATE TEMPORARY TABLE last_sync_date(schemaId VARCHAR, iban VARCHAR, lastSyncDate VARCHAR) ON COMMIT DROP;
   
    FOREACH _lastSyncDateFromFinnovaValiant IN ARRAY _listOfLastSyncDatesFromFinnovaValiant 
    LOOP
        SELECT string_to_array(_lastSyncDateFromFinnovaValiant, ';') INTO _lastSyncDateOfIban;
        _schema = _lastSyncDateOfIban[1];
        _valiantIban = _lastSyncDateOfIban[2];
        _lastSyncDate = _lastSyncDateOfIban[3];
       
        SELECT CONCAT(_bLinkLastSyncDateKeyPrefix, _valiantIban) into _bLinkLastSyncDateKey;
       
        -- Comment out this line if you want to see log at runtime
        -- RAISE NOTICE 'Schema - Valiant iban - lastSyncDate from Finnova: % - % - %', _schema, _valiantIban, _lastSyncDate;
       
        EXECUTE 'set schema ''' || _schema || '''';
    
        IF NOT EXISTS (SELECT 1 FROM key_value_store WHERE kv_key = _bLinkLastSyncDateKey) THEN 
            IF (_dryRunMode = 0) THEN
                INSERT INTO key_value_store(kv_key, kv_value) VALUES (_bLinkLastSyncDateKey, _lastSyncDate);
            END IF;
        
            INSERT INTO last_sync_date(schemaId, iban, lastSyncDate) VALUES (_schema, _valiantIban, _lastSyncDate);
        END IF;
       
    END LOOP;

    RETURN  query SELECT * FROM last_sync_date;
    RETURN;
END;
$BODY$
LANGUAGE plpgsql;

-- NOTE: Uncomment the line below if you want to printout the result to a file. Please change the directory below to your directory!!!
--COPY (SELECT * FROM insertLastSyncDatesFromValiantFinnovaToValiantBLink()) To 'C:\last_sync_dates_from_Finnova_inserted_into_bLink.csv' With CSV DELIMITER ',';

-- NOTE: Uncomment the line below if you want to printout the result in this screen
-- SELECT * FROM insertLastSyncDatesFromValiantFinnovaToValiantBLink(); 

-- Please run this command to delete the function after you finish
-- DROP FUNCTION insertLastSyncDatesFromValiantFinnovaToValiantBLink();
```

</div>

</div>

</div>

</div>

Example result:


![[47466315972-ceb6bbf8-f72d-4e53-b154-b3b1175c48f8.png]]
