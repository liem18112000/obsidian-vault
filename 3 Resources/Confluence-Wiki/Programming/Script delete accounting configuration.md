---
title: "Script delete accounting configuration"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZFIN/pages/20952598771/Script+delete+accounting+configuration
space: "LUZFIN"
topic: programming
relevance: 0.724
depth: 2.81
updated: 2017-11-03
attachments: 4
tags:
  - confluence
  - programming
  - space/luzfin
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
