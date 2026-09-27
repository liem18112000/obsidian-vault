---
title: "Bank connection - Script to store all old connected ibans for each tenant"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519726193/Bank+connection+-+Script+to+store+all+old+connected+ibans+for+each+tenant
space: "LUZ"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2020-12-31
attachments: 5
tags:
  - confluence
  - programming
  - space/luz
---

# Bank connection - Script to store all old connected ibans for each tenant

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-12-31 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519726193/Bank+connection+-+Script+to+store+all+old+connected+ibans+for+each+tenant)
> Relevance 0.731 · topic `programming`

Here is the groovy file containing all script to store all old connected ibans for each tenant in luz_keyvaluestore: [[20519726193-GET_ALL_OLD_CONNECTED_IBANS.groovy|GET_ALL_OLD_CONNECTED_IBANS.groovy]]

Regarding this script: it just collects ibans on tenants which are already connected to e-banking (Finnova, bLink, PostFinance). **The execution does not loop all tenants in the databases.**

Time consumed when running the script in DEV GCP is around less than 1s. In my opinion, when running this script on PROD, it takes less than 

There is some configurations to execute the groovy file by Postman:


![[20519726193-image2020-12-23_11-1-1.png]]



<div>

|  |  |
|----|----|
| key | value |
| file | groovy file |
| LUZ_FIN_FINANCE_DB | jdbc:<span rel="nofollow">postgresql://luz-database/luzfinance</span> |
| LUZ_FIN_FINANCE_USERNAME | username can access luz_finance db |
| LUZ_FIN_FINANCE_DB_PASSWORD | password can access luz_finance db |
| LUZ_FINNOVA_DB | jdbc:<span rel="nofollow">postgresql://luz-database/luzfinnova</span> |
| LUZ_FINNOVA_DB_USERNAME | username can access luz_finnova db |
| LUZ_FINNOVA_DB_PASSWORD | password can access luz_finnova db |
| LUZ_KEY_VALUE_STORE_DB | jdbc:<span rel="nofollow">postgresql://luz-database/luzkeyvaluestore</span> |
| LUZ_KEY_VALUESTORE_DB_USERNAME | username can access luz_key_value_store db |
| LUZ_KEY_VALUE_STORE_DB_PASSWORD | password can access luz_finance db |
| DRY_RUN_MODE | true/false |

</div>

After running the script with dry run mode, we can see how many tenants are affected based on bank types (picture below). Without dry run mode, all old connected ibans will be stored in key value store


![[20519726193-image2020-12-23_11-10-0.png]]



<span class="inline-comment-marker" ref="b198ebfc-fdea-4349-ab6b-8f91f76901c5">There is configuration if you use </span>**<span class="inline-comment-marker" ref="b198ebfc-fdea-4349-ab6b-8f91f76901c5">curl</span>**<span class="inline-comment-marker" ref="b198ebfc-fdea-4349-ab6b-8f91f76901c5">:</span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7b5e577d-382c-4ccc-afed-46b21525e0b2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# 1. terminal: kubectl run -i --tty --rm toolbox --image=gcr.io/klara-repo/toolbox --restart=Never -- /bin/bash
# 2. terminal: kubectl cp ~/Downloads/GET_ALL_OLD_CONNECTED_IBANS.groovy toolbox:/tmp/

# 1. terminal (inside toolbox)

export ADMINUSER=app
export ADMINPASS=app

export DBUSER=dbadmin
export DBPASS=dbadmin

curl --request POST --header "Content-Type:multipart/form-data" \
    -u "$ADMINUSER:$ADMINPASS" \
    -F LUZ_FIN_FINANCE_DB="jdbc:postgresql://luz-database/luzfinance" \
    -F LUZ_FIN_FINANCE_USERNAME="$DBUSER" \
    -F LUZ_FIN_FINANCE_DB_PASSWORD="$DBPASS" \
    -F LUZ_FINNOVA_DB="jdbc:postgresql://luz-database/luzfinnova" \
    -F LUZ_FINNOVA_DB_USERNAME="$DBUSER" \
    -F LUZ_FINNOVA_DB_PASSWORD="$DBPASS" \
    -F LUZ_KEY_VALUE_STORE_DB="jdbc:postgresql://luz-database/luzkeyvaluestore" \
    -F LUZ_KEY_VALUESTORE_DB_USERNAME="$DBUSER" \
    -F LUZ_KEY_VALUE_STORE_DB_PASSWORD="$DBPASS" \
    -F file=@"/tmp/GET_ALL_OLD_CONNECTED_IBANS.groovy" \
    -F DRY_RUN_MODE="true" \
    "luz-scripting-web:8080/luz_scripting_web/api/execute-groovy-script"
```

</div>

</div>

Here is the result: 


![[20519726193-image2020-12-23_11-48-31.png]]
