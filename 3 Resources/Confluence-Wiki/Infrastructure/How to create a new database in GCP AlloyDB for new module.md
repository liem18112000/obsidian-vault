---
ai_hash: edc8844e75dcbcda
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.701
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48871178255/How+to+create+a+new+database+in+GCP+AlloyDB+for+new+module
space: TS
status: reference
tags:
- confluence
- infra
- space/ts
title: How to create a new database in GCP AlloyDB for new module
topic: infra
type: source
updated: 2026-03-19
---

# How to create a new database in GCP AlloyDB for new module

> [!info] Imported from Confluence
> Space **TS** · updated 2026-03-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48871178255/How+to+create+a+new+database+in+GCP+AlloyDB+for+new+module)
> Relevance 0.701 · topic `infra`

This document is the renewed version of the old one [How to create new db for a module in GCP](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/47086436663/How+to+create+new+db+for+a+module+in+GCP)

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="1fa3a8e5-f2dd-42fb-8b05-28751c2eb6fd" macro-name="toc">

</div>

## Related modules:

- `luzfin_scripts`: add script to generate new database

- `luz_kubernetes`: generate secret for db

## Steps:

### Prepare database creation scripts in `luzfin_scripts`

1.  In package **luzfin_scripts/sql** add new `YYYY.MM.DD.00000_create_<dbName>_database/create_database.sql`

    For example: `2025.10.22.00000_create_luzmaubot_database/create_database.sql`

<div id="expander-786483113" class="expand-container conf-macro output-block" hasbody="true" macro-id="89e7a104-1511-45b8-ad47-698bf8cf18c5" macro-name="expand">

<div id="expander-control-786483113" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Detail content</span>

</div>

<div id="expander-content-786483113" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8479e963-6252-44b6-a5c4-1413f73ddf4a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
-- Create user role for luzmaubot
CREATE ROLE luzmaubot_USERNAME LOGIN
    PASSWORD 'luzmaubot_PASSWORD'
    INHERIT NOREPLICATION;

-- Grant current user ability to SET ROLE to luzmaubot_USERNAME
GRANT luzmaubot_USERNAME TO CURRENT_USER;

-- Create database
CREATE DATABASE luzmaubot
    WITH OWNER = luzmaubot_USERNAME
    ENCODING = 'UTF8'
    CONNECTION LIMIT = -1;

-- Grant privileges
GRANT CONNECT, TEMPORARY ON DATABASE luzmaubot TO public;
GRANT ALL ON DATABASE luzmaubot TO luzmaubot_USERNAME;

-- Grant permissions on public schema
\c luzmaubot
GRANT ALL ON SCHEMA public TO luzmaubot_USERNAME;
```

</div>

</div>

<div hasbody="true" macro-id="5b20c849-e358-4b36-a6e7-b6b0f066a70b" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Remember to replace the database’s name, database owner information

</div>

</div>

</div>

</div>

2.  In package **luzfin_scripts/runable** create a new file called `YYYY.MM.DD.00000_create_<dbName>_database.sh`  
    For example: `2025.10.22.00000_create_luzmaubot_database.sh`

<div id="expander-2126587704" class="expand-container conf-macro output-block" hasbody="true" macro-id="a55ef7c0-a2f1-4049-9dfd-d4b05a18dd59" macro-name="expand">

<div id="expander-control-2126587704" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Detail content</span>

</div>

<div id="expander-content-2126587704" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="044f1da7-729e-45b9-90e5-3b0f13dd4eae" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
#!/bin/sh
set -e

echo "START: creating luzmaubot database"

file_path="/var/app/data/sql/2025.10.22.00000_create_luzmaubot_database/create_database.sql"

sed -i -e "s/luzmaubot_USERNAME/$LUZ_MAUBOT_DB_USER/g" $file_path
sed -i -e "s/luzmaubot_PASSWORD/$LUZ_MAUBOT_DB_PASS/g" $file_path

PGPASSWORD=$ALLOYDB_PASSWORD psql -U "$ALLOYDB_USERNAME" -d postgres -h luz-alloydb-main -f $file_path

echo "END: created luzmaubot database"
```

</div>

</div>

<div hasbody="true" macro-id="d65e3aa1-0332-4d7c-a185-18aed6684534" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

- Remember to replace **your database’s name, database owner**

- These database owner info (e.g. `LUZ_MAUBOT_DB_USER`, `LUZ_MAUBOT_DB_PASS`) will be mapped with the generated secret in `luz_kubernetes`)

</div>

</div>

</div>

</div>

### Prepare database secrets in `luz_kubernetes`

1.  Create “creation shell script file” named `<your module>-create-env-secret.sh` in **luz_kubernetes/sops/scripts**  
    Example: <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/sops/scripts/luz-maubot-create-env-secret.sh" class="external-link" data-card-appearance="inline" data-local-id="1f165987-2743-469c-89a1-503316fd13b5" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/sops/scripts/luz-maubot-create-env-secret.sh</a>

2.  Using the created script to generate secrets for `dev-vn`, `dev`, `staging`

    - In **kubernetes-overlays/\<enviroment\>/\<your module\>/** create a **secret.properties (this file not commit in PR)** file with user, pass map to 2 above step’s records:  
      Example: kubernetes-overlays/env-dev/luz-maubot/secret.properties

      <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d7a07eec-efba-4e14-825c-4c86745da1fc" macro-name="code" style="border-width: 1px;">

      <div class="codeContent panelContent pdl">

      ``` syntaxhighlighter-pre
      LUZ_MAUBOT_DB_USER=luzmaubot
      LUZ_MAUBOT_DB_PASS=luzmaubot
      ```

      </div>

      </div>

    - Start the luz_deploy image into a container inside Docker desktop (start Docker Desktop if not started yet), linking it to the luz_kubernetes repo:  
      Run this command:

      <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f09cd634-d4c3-4f5e-a3c0-e1e806d7ab40" macro-name="code" style="border-width: 1px;">

      <div class="codeContent panelContent pdl">

      ``` syntaxhighlighter-pre
      docker run -it -v <your-project-kubernetes-location>:/root/development/luz_kubernetes europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-deploy:0.0.2 bash
      ```

      </div>

      </div>

    - Run the command to generate

      <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c2230d15-9ed9-4112-98ca-ecfad47aa96b" macro-name="code" style="border-width: 1px;">

      <div class="codeContent panelContent pdl">

      ``` syntaxhighlighter-pre
      ./<creation-shell-script-file> <enviroment> ../../kubernetes-overlays/<enviroment-folder>/luz-maubot/secret.properties
      ```

      </div>

      </div>

## Highlight

Please make sure your DB is connected by requiring sslmode

Example: <a href="https://bitbucket.org/axonivy-prod/luz_web_push_notification/src/99e5c01597b3ebb050c71fbacadeb08d21536d82/src/main/docker/Dockerfile.jvm?at=master#lines-97" class="external-link" data-card-appearance="inline" data-local-id="018c85297ee8" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_web_push_notification/src/99e5c01597b3ebb050c71fbacadeb08d21536d82/src/main/docker/Dockerfile.jvm?at=master#lines-97</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="49980fd8-5c38-43b2-ab67-f5c9376876e7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
ENV LUZ_WEBPUSHNOTIFICATION_JDBC jdbc:postgresql://luz-alloydb-main:5432/luzwebpushnotification?sslmode=require
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Deploy luz-epc-redis-service on GCP]]
- [[Setup Redis and DNS on TEST and PROD]]
- [[luz-vault - How to run Vault Benchmark]]
- [[EPC Notification]]
- [[ELM5 PubSub Message Queue]]

%% ai-graph-end %%