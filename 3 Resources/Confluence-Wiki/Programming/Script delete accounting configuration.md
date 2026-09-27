---
ai_hash: 9f4a5a66d911c119
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 2.81
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/LUZFIN/pages/20952598771/Script+delete+accounting+configuration
space: LUZFIN
status: reference
tags:
- confluence
- programming
- space/luzfin
title: Script delete accounting configuration
topic: programming
type: source
updated: 2017-11-03
---

# Script delete accounting configuration

> [!info] Imported from Confluence
> Space **LUZFIN** · updated 2017-11-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZFIN/pages/20952598771/Script+delete+accounting+configuration)
> Relevance 0.724 · topic `programming`

DO $$
       DECLARE rec RECORD; 
       BEGIN
           -- Get all the schemas
            FOR rec IN
      SELECT DISTINCT schemaname
      FROM pg_catalog.pg_tables
      -- You can exclude the schema which you don't want to drop by adding another condition here
      WHERE schemaname NOT IN ('pg_catalog', 'information_schema', 'public') 
            LOOP
      EXECUTE 'SET search_path TO ' || rec.schemaname;
        IF(EXISTS (SELECT * FROM information_schema.tables where table_schema = rec.schemaname and table_name='accounting_configuration')) then
          delete from accounting_configuration;
        end if;
        IF(EXISTS (SELECT * FROM information_schema.tables where table_schema = rec.schemaname and table_name='booking_detail')) then
          IF(EXISTS (SELECT * FROM information_schema.columns where table_schema = rec.schemaname and table_name='booking_header' and column_name='business_case')) then
          delete from booking_detail where booking_header_id IN (SELECT id from booking_header WHERE business_case = 'OPENING_BALANCE');
        end if;
        end if;
        IF(EXISTS (SELECT * FROM information_schema.tables where table_schema = rec.schemaname and table_name='booking_header')) then
          IF(EXISTS (SELECT * FROM information_schema.columns where table_schema = rec.schemaname and table_name='booking_header' and column_name='business_case')) then
          delete from booking_header where business_case = 'OPENING_BALANCE';
        end if;
        end if;
     END LOOP; 
       RETURN; 
    END;
    $$ LANGUAGE plpgsql;

%% ai-graph-start %%

**Related notes:**
- [[Fix migration issue DB Script Loop through tenant schema]]
- [[Delete company - Old way]]
- [[SQL script for populating master data]]
- [[Deactivate the Valiant Finnova interface]]
- [[Call API trigger Vacuum POS schema on PROD]]

%% ai-graph-end %%