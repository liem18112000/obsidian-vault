---
title: "Migration Script Execution Guide for luz-epc-api"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49224679427/Migration+Script+Execution+Guide+for+luz-epc-api
space: "LUZ"
topic: programming
relevance: 0.852
depth: 3
updated: 2026-03-12
attachments: 1
tags:
  - confluence
  - programming
  - space/luz
---

# Migration Script Execution Guide for luz-epc-api

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-03-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49224679427/Migration+Script+Execution+Guide+for+luz-epc-api)
> Relevance 0.852 · topic `programming`

<div hasbody="true" macro-id="0477df7f-5df0-42c2-a913-455673be6b9b" macro-name="tip">

<span class="aui-icon aui-icon-small aui-iconfont-approve confluence-information-macro-icon"> </span>

<div>


![[49224679427-1f680.png]]

 Release Steps – Migration Script for `luz-epc-api`

</div>

</div>

<div hasbody="true" macro-id="9c419783-a861-414f-ae51-bcca4c6c443d" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Purpose: Run the messagev1-to-messagev2 data migration safely in a Kubernetes environment while capturing logs for verification.

</div>

</div>

## Prerequisites

- kubectl context configured for the target cluster/namespace with exec permissions.

- Access to the `luz-epc-api` deployment in the desired namespace (e.g., dev/staging/prod).

- Sufficient pod resources to run a Node.js process alongside the service.

- HPA is fixed to 15 pods (prevent downscaling)

## Step 1: Execute Migration Script

Run the migration script inside the deployment pod. Redirect logs to a file so the process runs in the background:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c2df6101-1062-4830-87a1-6441ee4b0ebb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl exec deployment/luz-epc-api -n <env-namespace> -- node /app/migration/app.js > migration-output.log 2>&1 &
```

</div>

</div>

The process will start in the background.

- You’ll see a job ID confirmation (e.g., `[1] 1140`)

## Step 2: Monitor Logs (Optional – Initial Check)

You can tail the log file to verify that the migration has started:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6575c697-b71b-4b89-ae7c-24b2e3e9ecf3" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
tail -f migration-output.log
```

</div>

</div>

This step is optional.

- Once you confirm the migration is running, you may stop tailing and let it continue in the background.

## Step 3: Re-check Migration Progress

After some time, re-check the log file to monitor progress or confirm completion:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2969f434-d460-4b9d-ac64-559d14bae8eb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
tail -f migration-output.log
```

</div>

</div>

- Repeat this check as needed until the migration finishes.

- Look for success or error messages in the log output.

## Notes & Best Practices

- Do not interrupt the background process once started.

- Avoid multiple executions of the migration script unless required, as it may cause duplicate operations.

- Always review the final log output to confirm successful completion before proceeding with dependent tasks.

## Operational Tips

<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

If the container filesystem is read-only or logs must persist beyond pod lifecycle, redirect logs to stdout and capture via your cluster logging solution, or write to a mounted volume path (e.g., /var/log/migrations/migration-output.log).

</div>

</div>

## Verification Checklist

- <span class="placeholder-inline-tasks">Migration process started and background job ID observed</span>
- <span class="placeholder-inline-tasks">No errors found in migration-output.log</span>
- <span class="placeholder-inline-tasks">Final success message present in logs and data validated in v2 schema</span>
