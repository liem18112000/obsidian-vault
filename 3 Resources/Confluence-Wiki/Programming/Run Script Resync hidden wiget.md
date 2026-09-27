---
ai_hash: 981f4cf656052959
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 9
depth: 3
entities: []
relevance: 0.792
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47145617313/Run+Script+Resync+hidden+wiget
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Run Script Resync hidden wiget
topic: programming
type: source
updated: 2022-08-10
---

# Run Script Resync hidden wiget

> [!info] Imported from Confluence
> Space **Helios** · updated 2022-08-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47145617313/Run+Script+Resync+hidden+wiget)
> Relevance 0.792 · topic `programming`

**Step 1:** Port forward `luz_scripting_web` on PROD environment

Example on DEV: kubectl port-forward service/luz-scripting-web 8088:8080 -n dev

**Step 2:** Using postman or command line to call below curl

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f9cd8aa7-e34b-486a-a97c-221094138bf6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request POST 'http://localhost:8088/luz_scripting_web/api/execute-groovy-script' \
--header 'Authorization: Basic YWRtaW46YWRtaW4=' \
--form 'DB_USER="postgres"' \
--form 'DB_PASSWORD="postgres"' \
--form 'file=@"/C:/workspace/prod/luzfin_scripts/groovy/2022.07.19.00000_sync_hidden_subscriptions.groovy"' \
--form 'OUTPUT_FILE="/opt/luz-scripting-web/data/output/2022.07.19.00000_sync_hidden_subscriptions.csv"' \
--form 'SIZE="100"' \
--form 'DRY_RUN_MODE="false"' \
--form 'START_SUBSCRIPTION_ID="0"'
```

</div>

</div>

For Postman: Import the script as below:


![[47145617313-image-20220719-091836.png]]



**Step 3**: Download groovy file below

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="726a8fb9-de19-4384-8585-83d5d69859f1" macro-name="view-file"><a href="../_attachments/47145617313-2022.07.19.00000_sync_hidden_subscriptions.groovy" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47145617313/2022.07.19.00000_sync_hidden_subscriptions.groovy?version=3&amp;modificationDate=1660129314433&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[47145617313-2022.07.19.00000_sync_hidden_subscriptions.groovy]]

</a></span>

Select groovy file in Postman:


![[47145617313-image-20220719-092220.png]]

![[47145617313-image-20220719-091520.png]]



**Step 4:**

- Need to change “Basic auth” 's user name and password of PROD environment


![[47145617313-image-20220719-092330.png]]



**Tab Body definition:**

- `DB_USER`: Database Username on PROD

- `DB_PASSWORD`: Database Password on PROD

- `file`: The path to the script file, please download the groovy file and select it in postman (**Step 3**)

- `OUTPUT_FILE`: The path where the report file will be saved

- `SIZE`: The number of subscriptions will be affected in this run

- `DRY_RUN_MODE`:

  - true: run script with no data changed

  - false: realistic run

- `START_SUBSCRIPTION_ID`: Id of the subscription to start when the script runs

The first time will be 0, next time will be the last subscription id of the previous run + 1.

For example:

First run:

- `START_SUBSCRIPTION_ID`: 0

- Last subscription_id is synced in this run: 123

→ <span class="inline-comment-marker" ref="14c4be8f-74ef-4ac1-9731-3f8e4ff30a19">Next run </span><span class="inline-comment-marker" ref="14c4be8f-74ef-4ac1-9731-3f8e4ff30a19">`START_SUBSCRIPTION_ID`</span><span class="inline-comment-marker" ref="14c4be8f-74ef-4ac1-9731-3f8e4ff30a19"> = 123 + 1 = 124</span>

<div hasbody="true" macro-id="c3b9e84a-8039-4e0a-a094-9ac6cde1f67b" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

If this is the first run, to check that all thing work as expected we suggest running a small part first:

- SIZE: 100

- START_SUBSCRIPTION_ID: 0

- DRY_RUN_MODE: false

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Call API trigger Vacuum POS schema on PROD]]
- [[19. Sync POS indicators]]
- [[15. Update companies by tenant id]]
- [[18. Migrate indicator online_shop_5]]
- [[16. Export unsynchronized companies which missing from last synchronization]]

%% ai-graph-end %%