---
ai_hash: ef3f57b2af6025df
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 7
depth: 3
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47241199698/Call+API+trigger+Vacuum+POS+schema+on+PROD
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Call API trigger Vacuum POS schema on PROD
topic: programming
type: source
updated: 2022-12-20
---

# Call API trigger Vacuum POS schema on PROD

> [!info] Imported from Confluence
> Space **Helios** · updated 2022-12-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47241199698/Call+API+trigger+Vacuum+POS+schema+on+PROD)
> Relevance 0.786 · topic `programming`

Step 1: Create a **pos_schema_for_trigger_vacuum** table in the luz_pos database (public schema)

This table will contain a list of schema that need to be triggered vacuum, the data of this table will be read in the groovy script below, we will delete this table after we have triggered vacuum all schemas

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b1b44fd4-0240-4027-9715-df82b50e5a22" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
DO
$$
    DECLARE
        _schema text;
    BEGIN
        CREATE TABLE IF NOT EXISTS public.pos_schema_for_trigger_vacuum
        (
            tenant_id varchar(100)
        );
        FOR _schema IN
            SELECT nspname AS schema_name
            FROM pg_catalog.pg_namespace
            WHERE nspname LIKE 's_%'
            LOOP
                BEGIN
                    EXECUTE 'set schema ''' || _schema || '''';

                    EXECUTE 'INSERT INTO public.pos_schema_for_trigger_vacuum SELECT ''' || _schema ||
                            ''' WHERE EXISTS (select * from ' ||
                            _schema || '.transaction where transaction_date >= ''2022-01-01'')';
                EXCEPTION
                    WHEN OTHERS THEN NULL;
                END;
            END LOOP;
    END
$$;
```

</div>

</div>

**Step 2:** Port forward `luz_scripting_web` on the PROD environment

Example on DEV: kubectl port-forward service/luz-scripting-web 8088:8080 -n dev

**Step 3:** Using postman or the command line to call the below curl

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4281274f-3056-46df-9d28-9274d60ddbbc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request POST 'http://localhost:8088/luz_scripting_web/api/execute-groovy-script' \
--header 'Authorization: Basic YWRtaW46YWRtaW4=' \
--form 'DB_USER="postgres"' \
--form 'DB_PASSWORD="postgres"' \
--form 'file=@"/C:/workspace/prod/luzfin_scripts/groovy/2022.12.20.00000_TRIGGER_VACUUM_POS.groovy"' \
--form 'OUTPUT_FILE="/opt/luz-scripting-web/data/output/2022.12.20.00000_TRIGGER_VACUUM_POS.csv"' \
--form 'SIZE="10000"' \
--form 'DRY_RUN_MODE="false"' \
--form 'OFFSET="0"' \
--form 'USERNAME="admin"' \
--form 'PASSWORD="admin"' \
--form 'SPECIFIC_SCHEMAS="s_f1cc489f_12eb_43f8_b11b_c1eefd8cc70e;s_effe0740_0f90_4c86_b08f_f7f18b5f6ac7;s_0bb4936f_0c99_4b84_a6d5_156917a94872"'
```

</div>

</div>

For Postman: Import the script as below:


![[47241199698-image-20221220-083457.png]]



**Step 4**: Download the groovy file below

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="4db95fc8-504a-4634-9df3-e7af1a07796a" macro-name="view-file"><a href="../_attachments/47241199698-2022.12.20.00000_TRIGGER_VACUUM_POS.groovy" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47241199698/2022.12.20.00000_TRIGGER_VACUUM_POS.groovy?version=1&amp;modificationDate=1671525344936&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[47241199698-2022.12.20.00000_TRIGGER_VACUUM_POS.groovy]]

</a></span>

Select a groovy file in Postman:


![[47241199698-image-20221220-083622.png]]



**Step 5:**

- Need to change “Basic auth” 's user name and password of PROD environment

This is the user **system admin of Klara**, the value is the decoded value of 2 secrets **CRON_USER** and **CRON_PASSWORD.**


![[47241199698-image-20220719-092330.png]]



**Tab Body definition:**

- `DB_USER`: Database Username on PROD (user can do DML query)

- `DB_PASSWORD`: Database Password on PROD

- `file`: The path to the script file, please download the groovy file and select it in postman (**Step 4**)

- `OUTPUT_FILE`: The path where the result file will be saved, if the filename already exists then that file will be overwritten

check the output file:

- file path (module: luz_scripting_web)

/opt/luz-scripting-web/data/output/{{output file name}}

- `SIZE`: The number of schemas will be affected in this run, if null then trigger all schemas

- `DRY_RUN_MODE`:

  - true: run script with no data changed

  - false: realistic run

- `OFFSET`: specifies the number of tenants to skip, if null then offset = 0

- `SPECIFIC_SCHEMAS`: specifies the schemas to trigger, if null then get from the database.

After running this API, below is the response


![[47241199698-image-20221220-091930.png]]



example log in luz_scripting_web


![[47241199698-image-20221220-091709.png]]



Example result file in luz_scripting_web:


![[47241199698-image-20221220-091844.png]]



You can use the next offset value in the output file for the next run.

%% ai-graph-start %%

**Related notes:**
- [[How to execute API to create sync event for post from tenant schemas to public table]]
- [[Run Script Resync hidden wiget]]
- [[19. Sync POS indicators]]
- [[11. Collect and write out companies were verified their business to Hubspot]]
- [[15. Update companies by tenant id]]

%% ai-graph-end %%