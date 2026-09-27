---
ai_hash: 59fcb7c4188afe57
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.757
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20490587885/Delete+company+-+Old+way
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Delete company - Old way
topic: programming
type: source
updated: 2021-11-16
---

# Delete company - Old way

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-11-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20490587885/Delete+company+-+Old+way)
> Relevance 0.757 · topic `programming`

### <span class="legacy-color-text-red2">NOTES: Use ONLY for development environments</span>

## Configure

Groovy script to delete company by tenant id.

### Method type & Endpoint

<div>

|  |  |
|----|----|
| <span class="legacy-color-text-default">Method</span> | <span class="legacy-color-text-default">POST</span> |
| <span class="legacy-color-text-default">Endpoint</span> | <span class="legacy-color-text-default">http://\<host\>:8080/luz_scripting_web/api/execute-groovy-script</span> |

</div>

### <span class="legacy-color-text-default">Authorization</span>

<div>

|  |  |
|----|----|
| <span class="legacy-color-text-default">Basic Auth</span> | <span class="legacy-color-text-default">username/password (ex: admin/admin)</span> |

</div>

Or **Header **

<div>

|  |  |
|----|----|
| <span class="legacy-color-text-default">Authorization</span> | Basic a2xlZS1hZG1pbjphZG1pbkBrbGV |

</div>

### <span class="legacy-color-text-default">Body (form-data)</span>

<div>

|  |  |
|----|----|
| <span class="legacy-color-text-default">file</span> | **luzfin_scripts: <a href="https://bitbucket.org/axonivy-prod/luzfin_scripts/src/master/src/main/resources/com/axonivy/company/delete_company.groovy" class="external-link" rel="nofollow" style="text-decoration: none;text-align: left;"><span>src<span class="css-5hz1ob e1rbit5u0">/</span>main<span class="css-5hz1ob e1rbit5u0">/</span>resources<span class="css-5hz1ob e1rbit5u0">/</span>com<span class="css-5hz1ob e1rbit5u0">/</span>axonivy<span class="css-5hz1ob e1rbit5u0">/</span>company<span class="css-5hz1ob e1rbit5u0">/</span></span>delete_company.groovy</a>** |
| TENANT_ID | c398b955-efaf-45a0-900c-38f96b86ee5e |
| <span class="legacy-color-text-default">DB_USER</span> | postgres |
| <span class="legacy-color-text-default">DB_PASSWORD</span> | postgres |

</div>

  
**POSTMAN json example**

Json file: [[20490587885-delete_company_postman.json|delete_company_postman.json]]


![[20490587885-image2019-7-26_17-42-46.png]]



### Wildfly Log  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a464c073-c8f0-4fa6-b092-6cde638979f9" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Log when execute script**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
2019-07-08 06:53:58,529 INFO [QueryDBNamesCommand] (default task-85) Connected to database: postgres
2019-07-08 06:53:58,558 INFO [QueryDBNamesCommand] (default task-85) Disconnected from database: postgres
2019-07-08 06:53:58,562 INFO [stdout] (default task-85) ---------list-databases--------[luzperson:luzperson, luzstore:luz_store, luzcreditsuisse:luzcreditsuisse, luzkeyvaluestore:luzkeyvaluestore, auth:auth, elm:elm, luzimmo:luz_immo, luzdocmanager:luzdocmanager, filemanager:filemanager, ivysystemdb721:ivysystemdb_7_2_1, luztenant:luztenant, luzarticle:luz_article, klee:klee, luzcompensation:luzcompensation, luzaccounting:luzaccounting, luzpos:luz_pos, luzfinnova:luz_finnova, luzpayment:luzpayment, luzsystem:luzsystem, postgres:postgres, luzfinance:luzfinance]
2019-07-08 06:53:58,589 INFO [FileManagerDeleteCommand] (default task-85) Connected to database: filemanager
2019-07-08 06:53:58,635 INFO [FileManagerDeleteCommand] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,635 INFO [FileManagerDeleteCommand] (default task-85) Disconnected from database: filemanager
2019-07-08 06:53:58,639 INFO [LuzSystemDeleteCommand] (default task-85) Connected to database: luzsystem
2019-07-08 06:53:58,646 INFO [LuzSystemDeleteCommand] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e in public
2019-07-08 06:53:58,648 INFO [LuzSystemDeleteCommand] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,648 INFO [LuzSystemDeleteCommand] (default task-85) Disconnected from database: luzsystem
2019-07-08 06:53:58,651 INFO [LuzTenantDeleteCommand] (default task-85) Connected to database: luztenant
2019-07-08 06:53:58,674 INFO [LuzTenantDeleteCommand] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,674 INFO [LuzTenantDeleteCommand] (default task-85) Disconnected from database: luztenant
2019-07-08 06:53:58,678 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: elm
2019-07-08 06:53:58,678 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,678 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: elm
2019-07-08 06:53:58,681 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: klee
2019-07-08 06:53:58,682 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,682 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: klee
2019-07-08 06:53:58,685 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luzaccounting
2019-07-08 06:53:58,685 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,685 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luzaccounting
2019-07-08 06:53:58,688 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luz_article
2019-07-08 06:53:58,689 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,689 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luz_article
2019-07-08 06:53:58,692 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luzcompensation
2019-07-08 06:53:58,765 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,765 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luzcompensation
2019-07-08 06:53:58,768 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luzdocmanager
2019-07-08 06:53:58,769 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,769 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luzdocmanager
2019-07-08 06:53:58,772 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luzfinance
2019-07-08 06:53:58,815 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,815 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luzfinance
2019-07-08 06:53:58,818 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luz_finnova
2019-07-08 06:53:58,818 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,819 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luz_finnova
2019-07-08 06:53:58,821 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luzperson
2019-07-08 06:53:58,853 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,853 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luzperson
2019-07-08 06:53:58,856 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luz_pos
2019-07-08 06:53:58,856 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,857 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luz_pos
2019-07-08 06:53:58,859 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luz_store
2019-07-08 06:53:58,860 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,860 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luz_store
2019-07-08 06:53:58,863 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luzcreditsuisse
2019-07-08 06:53:58,864 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,864 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luzcreditsuisse
2019-07-08 06:53:58,867 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luzpayment
2019-07-08 06:53:58,867 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,867 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luzpayment
2019-07-08 06:53:58,870 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luzkeyvaluestore
2019-07-08 06:53:58,870 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,870 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luzkeyvaluestore
2019-07-08 06:53:58,873 INFO [DeleteCommandByDropSchema] (default task-85) Connected to database: luz_immo
2019-07-08 06:53:58,873 INFO [DeleteCommandByDropSchema] (default task-85) Deleted schema s_c398b955_efaf_45a0_900c_38f96b86ee5e
2019-07-08 06:53:58,873 INFO [DeleteCommandByDropSchema] (default task-85) Disconnected from database: luz_immo
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[15. Update companies by tenant id]]
- [[14. Create companies by tenant id]]
- [[LUZ-102045 Implement physical delete for COMPANY tenant Part 2 (Postgres cont)]]
- [[16. Export unsynchronized companies which missing from last synchronization]]
- [[Bank connection - Script to store all old connected ibans for each tenant]]

%% ai-graph-end %%