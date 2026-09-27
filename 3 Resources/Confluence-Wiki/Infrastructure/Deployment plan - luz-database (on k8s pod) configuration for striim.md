---
title: "Deployment plan - luz-database (on k8s pod) configuration for striim"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/IO/pages/48665067609/Deployment+plan+-+luz-database+on+k8s+pod+configuration+for+striim
space: "IO"
topic: infra
relevance: 0.76
depth: 3
updated: 2025-10-02
attachments: 0
tags:
  - confluence
  - infra
  - space/io
---

# Deployment plan - luz-database (on k8s pod) configuration for striim

> [!info] Imported from Confluence
> Space **IO** · updated 2025-10-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/IO/pages/48665067609/Deployment+plan+-+luz-database+on+k8s+pod+configuration+for+striim)
> Relevance 0.76 · topic `infra`

Configure luz-database for striim.

Pre-conditions:

- snapshot

- possible to return to wal_level = minimal

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Time</strong></p></th>
<th></th>
<th></th>
</tr>
&#10;<tr>
<td><p>15:00</p></td>
<td><p>Delete luz-database service<br />
- luz-database<br />
- luz-database-lb</p></td>
<td><p>Avoid that the db get spammed with requests</p></td>
</tr>
<tr>
<td></td>
<td><p>Scale down all modules with databases via script/kubectl</p></td>
<td><p>We have to restart, so they can reconnect, otherwise stale connection</p></td>
</tr>
<tr>
<td><p>15:15</p></td>
<td><p>Stop luz-database</p></td>
<td><p>check in logs for “database system is shut down” ← proper shutdown</p></td>
</tr>
<tr>
<td><p>15:20</p></td>
<td><p>create snapshot</p></td>
<td><p>name - snapshot-before-striim-reconfiguration-performance-db-primary</p></td>
</tr>
<tr>
<td><p>15:30</p></td>
<td><p>luz-database/configs commented in env-performance/kustomization.yaml on master branch</p></td>
<td><p>We must disallow the old configuration to take place</p></td>
</tr>
<tr>
<td><p>15:45</p></td>
<td><p>deploy luz-database from Mischa-branch</p>
<p>Do configuration</p></td>
<td></td>
</tr>
<tr>
<td><p>16:15</p></td>
<td><p>Start luz-database</p></td>
<td></td>
</tr>
<tr>
<td><p>16:20</p></td>
<td><p>luz-database has started</p></td>
<td></td>
</tr>
<tr>
<td><p>16:20</p></td>
<td><p>Check db configuration</p></td>
<td><p>Does the db itself work ?<br />
Check striim</p></td>
</tr>
<tr>
<td><p>16:30</p></td>
<td><p>Recreate luz-database service</p></td>
<td></td>
</tr>
<tr>
<td><p>17:00</p></td>
<td><p>Test application</p></td>
<td></td>
</tr>
<tr>
<td><p>17:30</p></td>
<td><p>Inform PO/SM/Invisible</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Recovery if db does not startup after Mischa-branch deployment</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>15:00</p></td>
<td><p>Stop luz-database</p></td>
<td><p>the modules are still down</p></td>
</tr>
<tr>
<td><p>15:00</p></td>
<td><p>Make disk from snapshot<br />
Adjust luz-database configuration (-&gt; new disk)<br />
Enable all configs</p></td>
<td></td>
</tr>
<tr>
<td><p>16:00</p></td>
<td><p>Deploy luz-database</p></td>
<td></td>
</tr>
<tr>
<td><p>16:30</p></td>
<td><p>luz-database is started</p></td>
<td></td>
</tr>
<tr>
<td><p>16:30</p></td>
<td><p>Check database</p></td>
<td></td>
</tr>
<tr>
<td><p>17:00</p></td>
<td><p>Scale up all modules with databases via script/kubectl</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Recovery if striim causes problems</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/people/712020:537ab411-930d-4772-b445-3f61570296b9?ref=confluence" class="confluence-userlink user-mention" data-account-id="712020:537ab411-930d-4772-b445-3f61570296b9" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Mykhaylo Ilchenko</a><br />
pls fill out and lets double check</p></td>
<td></td>
</tr>
<tr>
<td><p>15:00</p></td>
<td><p>delete replica slots</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>
