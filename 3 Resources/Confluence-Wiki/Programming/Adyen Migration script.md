---
title: "Adyen Migration script"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48888119542/Adyen+Migration+script
space: "Helios"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2025-12-12
attachments: 4
tags:
  - confluence
  - programming
  - space/helios
---

# Adyen Migration script

> [!info] Imported from Confluence
> Space **Helios** · updated 2025-12-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48888119542/Adyen+Migration+script)
> Relevance 0.738 · topic `programming`

From this story <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48888119542_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-143639" macro-id="912358da-0d90-44b0-a2ef-5e1997f2955a" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-143639" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-143639</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> we have the script to migrate the past payment data on luz-adyen.

This is a script on the repository <a href="https://bitbucket.org/axonivy-prod/luzfin_scripts/src/master/groovy/2025.11.18.00000_trigger_migration_the_past_payment_for_luz_adyen.groovy" class="external-link" data-card-appearance="inline" data-local-id="002e08bf-a83b-491f-addd-bf3107cbc347" rel="nofollow">https://bitbucket.org/axonivy-prod/luzfin_scripts/src/master/groovy/2025.11.18.00000_trigger_migration_the_past_payment_for_luz_adyen.groovy</a> or get directly <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="459c894f-5b4a-4fe8-8bf1-5bd24c8536ec" macro-name="view-file"><a href="../_attachments/48888119542-2025.11.18.00000_trigger_migration_the_past_payment_for_luz_adyen.groovy" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/48888119542/2025.11.18.00000_trigger_migration_the_past_payment_for_luz_adyen.groovy?version=1&amp;modificationDate=1763718445146&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[48888119542-2025.11.18.00000_trigger_migration_the_past_payment_for_luz_adyen.groovy]]

</a></span>

### 1. Execute the script:

As usual, you will receive the file and execute it through Luz-Scripting-Web.

- curl:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6ed5131d-1323-43e7-a96a-73e9ee33071e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --silent --location --request POST 'localhost:8080/luz_scripting_web/api/execute-groovy-script' \
--header 'Authorization: Basic YWRtaW46YWRtaW4=' \
--form 'file=@"/Users/nvhau/Projects/Klaras/luzfin_scripts/groovy/2025.11.18.00000_trigger_migration_the_past_payment_for_luz_adyen.groovy"' \
--form 'FROM_DATE="2025-10-28"' \
--form 'TO_DATE="2025-11-03"' \
--form 'CRON_USER=""' \
--form 'CRON_PASS=""' 
```

</div>

</div>

- Please update the correct value for the Cron user/pwd, also the login credentials


![[48888119542-Screenshot 2025-11-21 at 16.37.35-20251121-093738.png]]



### 2. What does the script do:

This script automates the migration of past payment data for the luz-adyen system.

The business process it supports is:

1.  Securely authenticate using cron job credentials to obtain a JWT token.

2.  Trigger a payment migration for a specified date range (FROM_DATE to TO_DATE) by calling the luz-adyen API. The luz-adyen service will trigger the Adyen payment migration process for each day within the FROM_DATE and TO_DATE range via a Pub/Sub message. It will then save the payment data into the luz-adyen database.

3.  Log the migration process and return a structured response indicating success, failure, or error.

### 3. Parameters

<div>

|  |  |  |  |
|----|----|----|----|
| **Parameter** | **Require** | **Example** | **Description** |
| file | 

![[48888119542-check.png]]

 | {path}/2025.11.18.00000_trigger_migration_the_past_payment_for_luz_adyen.groovy | the groovy script |
| FROM_DATE | 

![[48888119542-check.png]]

 | 2025-07-07 | Start date for payment migration (inclusive) (e.g., 2025-07-07) |
| TO_DATE | 

![[48888119542-check.png]]

 | 2025-07-21 | End date for payment migration (inclusive) (e.g., 2025-07-21) |
| CRON_USER | 

![[48888119542-check.png]]

 |  | CRON_USERNAME |
| CRON_PASS | 

![[48888119542-check.png]]

 |  | CRON_PASSWORD |

</div>

### 4. Output

This script currently only triggers the migration signal process on luz-adyen; the migration status will be updated in the luz-adyen logs.  
**Please inform us after you have executed the script so we can observe and analyze the logs afterward.**
