---
ai_hash: 13af3a850ab7671a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47482700524/Fix+migration+issue+DB+Script+Loop+through+tenant+schema
space: HACKA
status: reference
tags:
- confluence
- programming
- space/hacka
title: 'Fix migration issue: DB Script Loop through tenant schema'
topic: programming
type: source
updated: 2026-03-18
---

# Fix migration issue: DB Script Loop through tenant schema

> [!info] Imported from Confluence
> Space **HACKA** · updated 2026-03-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47482700524/Fix+migration+issue+DB+Script+Loop+through+tenant+schema)
> Relevance 0.786 · topic `programming`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3ae724a1-9be6-49a4-98f7-8a66e7c91e66" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
DO
$$
DECLARE final_statement text;
DECLARE script_name TEXT;
DECLARE schema TEXT;
DECLARE count_value INT8;
BEGIN
    script_name := '''V2022.01.04.00000__CREATE_TABLE_INVOICE_NUMBER.sql''';
    FOR schema IN SELECT nspname AS schema_name FROM pg_catalog.pg_namespace WHERE nspname LIKE 's_%' ORDER BY nspname
    LOOP
        BEGIN       
            --final_statement := 'DELETE FROM ' || schema || '.schema_version' || ' WHERE ' || schema || '.schema_version.script = ' || script_name;            
            --EXECUTE final_statement ;                     
            
            final_statement := 'SELECT COUNT(*) FROM ' || schema || '.schema_version' || ' WHERE ' || schema || '.schema_version.script = ' || script_name;         
            EXECUTE final_statement into count_value;           
            IF count_value > 0 THEN
                raise notice '% count migration in the schema: %', count_value, schema;
            END IF;     
            
        EXCEPTION               
            WHEN undefined_table then RAISE NOTICE 'table schema_version is undefined';
        END;
        
    END LOOP;
END;
$$
```

</div>

</div>

Use the above loop to fix Flyway migration issue “*Validate failed: Migration checksum mismatch for migration version*“.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ec50d865-83fe-4819-abda-c277af34b476" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Validate failed: Migration checksum mismatch for migration version 2026.01.22.00000
```

</div>

</div>

Delete table  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="675e4044-ae96-4720-8061-995a131a9824" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
DO
$$
DECLARE final_statement text;
DECLARE script_name TEXT;
DECLARE schema TEXT;
DECLARE count_value INT8;
BEGIN
    script_name := '''V2026.03.08.00000__CREATE_TABLE_BUSINESS_STATUS_HISTORY.sql''';
    FOR schema IN SELECT nspname AS schema_name FROM pg_catalog.pg_namespace WHERE nspname LIKE 's_%' ORDER BY nspname
    LOOP
        BEGIN       
            final_statement := 'DELETE FROM ' || schema || '.schema_version' || ' WHERE ' || schema || '.schema_version.script = ' || script_name;          
            EXECUTE final_statement ;                       

            final_statement := 'DROP TABLE IF EXISTS ' || schema || '.business_status_history CASCADE;';
            EXECUTE final_statement;
            
            --add drop table {schema}.business_status_history
            
            --final_statement := 'SELECT COUNT(*) FROM ' || schema || '.schema_version' || ' WHERE ' || schema || '.schema_version.script = ' || script_name;           
            --EXECUTE final_statement into count_value;         
            IF count_value > 0 THEN
                raise notice '% count migration in the schema: %', count_value, schema;
            END IF;     
            
        EXCEPTION               
            WHEN undefined_table then RAISE NOTICE 'table schema_version is undefined';
        END;
        
    END LOOP;
END;
$$
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Script delete accounting configuration]]
- [[Script to add hibernate_sequence for new Klara customers]]
- [[SQL Script execution]]
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]
- [[Deactivate the Valiant Finnova interface]]

%% ai-graph-end %%