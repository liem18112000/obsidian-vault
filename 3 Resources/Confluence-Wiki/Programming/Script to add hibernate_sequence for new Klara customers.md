---
ai_hash: 48da17eefaba7b0f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.886
source: https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/23195435760/Script+to+add+hibernate_sequence+for+new+Klara+customers.
space: NEXT
status: reference
tags:
- confluence
- programming
- space/next
title: Script to add "hibernate_sequence" for new Klara customers.
topic: programming
type: source
updated: 2020-06-24
---

# Script to add "hibernate_sequence" for new Klara customers.

> [!info] Imported from Confluence
> Space **NEXT** · updated 2020-06-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/23195435760/Script+to+add+hibernate_sequence+for+new+Klara+customers.)
> Relevance 0.886 · topic `programming`

**PLEASE DO NOT USE THE SCRIPT AT THE MOMENT**

Here is the script to add "hibernate_sequence" for add hibernate_sequence

**Note: With the script below, you can run for all tenants ( it only adds hibernate_sequence for tenants which don't have a hibernate_sequence. If you only want to run for specific tenant(s), un-comment AND-condition and add the tenant(s) you want inside ''.**

**THIS IS A CORRECT SCRIPT**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="691657d9-99d4-4b69-bbb0-605affe3151b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
DO $$DECLARE
_schema text;
_final_statement text;
BEGIN
FOR _schema IN
SELECT distinct quote_ident(nspname)
FROM pg_catalog.pg_class c
JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
WHERE nspname !~~ 'pg_%'
    AND nspname <> 'information_schema'
    AND nspname <> 'public'
-- IF you want to check specific tenant, please un-comment AND-condition below and replace the tenants as you wish. For example: AND nspname IN ('s_642247b0_7e78_4a92_9f2a_74b727684732'), default: run all tenants and add a sequence if not having
 -- AND nspname IN ('tenantId')
LOOP
    raise notice 'from the schema: %', _schema;
    _final_statement := 'create SEQUENCE IF NOT EXISTS ' || _schema || '.hibernate_sequence start 1 increment 1' ;
                    EXECUTE _final_statement;
END LOOP;
END$$;
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[SQL Script execution]]
- [[Fix migration issue DB Script Loop through tenant schema]]
- [[Call API trigger Vacuum POS schema on PROD]]
- [[Script to list all the information of the tenants]]
- [[Script to create task again for banks using b.Link]]

%% ai-graph-end %%