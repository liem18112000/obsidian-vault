---
title: "Payout Migration script"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49130831873/Payout+Migration+script
space: "Helios"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2026-02-09
attachments: 7
tags:
  - confluence
  - programming
  - space/helios
---

# Payout Migration script

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-02-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49130831873/Payout+Migration+script)
> Relevance 0.738 · topic `programming`

From this task <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49130831873_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-147291" macro-id="52f117d4-c074-42dc-98b6-3d8eabe25d27" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-147291" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-147291</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> , we have the script to migrate the payout data on luz-adyen.

This is a script in the repository <a href="https://bitbucket.org/axonivy-prod/luzfin_scripts/src/master/groovy/2026.02.09.00000_migration_past_payout_report_by_tenant.groovy" class="external-link" data-card-appearance="inline" data-local-id="4292ac732538" rel="nofollow">https://bitbucket.org/axonivy-prod/luzfin_scripts/src/master/groovy/2026.02.09.00000_migration_past_payout_report_by_tenant.groovy</a> , or get it directly <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="c4da47fb-3f0f-4f3f-bbd2-e35a0c6c7a15" macro-name="view-file"><a href="../_attachments/49130831873-2026.02.09.00000_migration_past_payout_report_by_tenant.groovy" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/49130831873/2026.02.09.00000_migration_past_payout_report_by_tenant.groovy?version=2&amp;modificationDate=1770625122534&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/x-c++" data-has-thumbnail="true">

![[49130831873-2026.02.09.00000_migration_past_payout_report_by_tenant.groovy]]

</a></span>

### 1. Execute the script:

As usual, you will receive the file and execute it through Luz-Scripting-Web.

- curl:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="679dd302-17c8-42ad-abb7-aa7d9d59f515" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --silent --location --request POST 'localhost:8080/luz_scripting_web/api/execute-groovy-script' \
--header 'Authorization: Basic YWRtaW46YWRtaW4=' \
--form 'file=@"/Users/nvhau/Projects/Klaras/luzfin_scripts/groovy/2026.02.09.00000_migration_past_payout_report_by_tenant.groovy"' \
--form 'TENANT_DATA=@"/Users/nvhau/Projects/Klaras/AdyenData/PROD Issues/a1f3cbb3_767b_4c0b_ae6b_1b340181863c_PayoutIncludeWrongPayments/PayoutMigration.json"' \
--form 'LUZ_ADYEN_DB_USER=""' \
--form 'LUZ_ADYEN_DB_PASS=""' \
--form 'CRON_USER=""' \
--form 'CRON_PASSWORD=""'
```

</div>

</div>

- Please update the correct values for TENANT_DATA, CRON_USER, CRON_PASSWORD, LUZ_ADYEN_DB_USER, LUZ_ADYEN_DB_PASS, also the login credentials


![[49130831873-Screenshot 2026-02-09 at 15.21.54-20260209-082155.png]]



### 2. What does the script do:

This script will remove the duplicate payments that match the transferId defined in the script

This script automates the migration of payout payment data on 2025-11-17 for the tenants on the Luz-Adyen system.

The business process it supports is:

1.  Remove the duplicate payment entities based on the transferId defined in the script.

2.  Securely authenticate using cron job credentials to obtain a JWT token.

3.  Trigger a payout migration for a specified tenant’s data by calling the Luz-Adyen API. The Luz-Adyen service will process payout migration and save data into the Luz-Adyen database.

4.  Log the migration process and return a structured response indicating success, failure, or error.

### 3. Parameters

<div>

|  |  |  |  |
|----|----|----|----|
| **Parameter** | **Require** | **Example** | **Description** |
| file | 

![[49130831873-check.png]]

 | {path}/2026.02.09.00000_migration_past_payout_report_by_tenant.groovy | the groovy script |
| TENANT_DATA |  | {path}/PayoutMigration.json | PayoutMigration.json file is provided in the email. |
| LUZ_ADYEN_DB_USER | 

![[49130831873-check.png]]

 |  | LUZ_ADYEN_DB_USER |
| LUZ_ADYEN_DB_PASS | 

![[49130831873-check.png]]

 |  | LUZ_ADYEN_DB_PASS |
| CRON_USER | 

![[49130831873-check.png]]

 |  | CRON_USERNAME |
| CRON_PASSWORD | 

![[49130831873-check.png]]

 |  | CRON_PASSWORD |

</div>

### 4. Output

Please inform us of the result.
