---
title: "SQL Script execution"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AVATAR/pages/6459228727/SQL+Script+execution
space: "AVATAR"
topic: programming
relevance: 0.724
depth: 2.81
updated: 2021-01-27
attachments: 11
tags:
  - confluence
  - programming
  - space/avatar
---

# SQL Script execution

> [!info] Imported from Confluence
> Space **AVATAR** · updated 2021-01-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/AVATAR/pages/6459228727/SQL+Script+execution)
> Relevance 0.724 · topic `programming`

Regarding to story  <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_6459228727_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-49936" macro-id="6ef54e4b-b577-4e7f-ad6b-48e051df689f" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-49936" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-49936</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>  , we create 2 scripts to get data.

#### Count all tenant

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d4bcfb9d-b07b-4e43-881c-867e7a0bd9ec" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**luztenant**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT COUNT(id) AS total_tenant
FROM public.tenant tenant
WHERE tenant.type = 'company-tenant'
```

</div>

</div>

on **klara-dev**

#### 

![[6459228727-image2021-1-27_11-36-47.png]]



on **dev.klara.ch**


![[6459228727-image2021-1-27_11-44-40.png]]



#### Count all customer of all tenant

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c1ad8f5c-5fbc-48f7-a171-c3b183766043" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**luzfinance**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
DO $$
 DECLARE schema TEXT;
 DECLARE counter_person INTEGER;
 DECLARE counter_company INTEGER;
 DECLARE total_customer_person INTEGER = 0 ;
 DECLARE total_customer_company INTEGER = 0 ;
 BEGIN
 -- Get all the schemas
    FOR schema IN SELECT nspname AS schema_name FROM pg_catalog.pg_namespace WHERE nspname LIKE 's_%'
    LOOP
        BEGIN
            EXECUTE 'SET search_path TO ' || schema;
            SELECT INTO counter_person COUNT(*) FROM customer WHERE customer.type = 'PERSON';
            total_customer_person =  total_customer_person + counter_person;
            SELECT INTO counter_company COUNT(*) FROM customer WHERE customer.type = 'COMPANY';
            total_customer_company =  total_customer_company + counter_company;
        EXCEPTION
            WHEN others THEN RAISE NOTICE 'Customer table does not exist.';
        END;

 
 END LOOP;
 RAISE NOTICE 'Total number of CRM customer/partners (person) %. ',total_customer_person;
 RAISE NOTICE 'Total number of CRM customer/partners (company) %. ',total_customer_company;
 RETURN;
END;
$$ LANGUAGE PLPGSQL;
```

</div>

</div>

on **klara-dev**


![[6459228727-image2021-1-27_15-55-32.png]]



on **dev.klara.ch**


![[6459228727-image2021-1-27_15-57-22.png]]
