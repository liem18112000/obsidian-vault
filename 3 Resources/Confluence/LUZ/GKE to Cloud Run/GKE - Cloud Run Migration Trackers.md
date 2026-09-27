---
ai_hash: 33441d251c144336
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49300078635'
confluence_path: LUZ Home > GKE to Cloud Run
created: 2026-04-06
entities: []
source: Confluence · LUZ - LUZ
status: reference
tags:
- confluence
- cloud-run
title: '[GKE - Cloud Run] Migration Trackers'
type: source
updated: 2026-05-19
url: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49300078635/GKE+-+Cloud+Run+Migration+Trackers
---

# [GKE - Cloud Run] Migration Trackers

*Confluence source · LUZ Home › GKE to Cloud Run · [view original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49300078635/GKE+-+Cloud+Run+Migration+Trackers) · updated 2026-05-19*

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p>**Team**</p></th>
<th><p>**Status**</p></th>
<th><p>**GKE Service Name**</p></th>
<th><p>**GKE Secrets**</p></th>
<th><p>**GCP Secret Manager**</p></th>
<th><p>**Dependencies (DB, Pub/Sub, FileStore, etc.)**</p></th>
<th><p>**Cloud Run Service**</p></th>
<th><p>**VPC**</p></th>
<th><p>**Note**</p></th>
<th><p>**References**</p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Kepler</p></td>
<td><p>`RELEASED PROD`</p></td>
<td><p>luz-antivirus</p></td>
<td><p>-</p></td>
<td><p>-</p></td>
<td><ul>
<li><p>Google Cloud Build</p></li>
</ul></td>
<td><p>`{env}-luz-antivirus-{project-id}.{region}.run.app`</p>
<p>Example: `https://dev-luz-antivirus-335505349498.europe-west6.run.app`</p></td>
<td><p>Serverless connector</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><p>Helios</p></td>
<td><p>`RELEASED PROD`</p></td>
<td><p>luz-message-broker</p></td>
<td><ul>
<li><p>run-as-token-env-secret</p></li>
<li><p>pubsub-adminsdk-certificate</p></li>
</ul></td>
<td></td>
<td></td>
<td><p>`{env}-luz-message-broker-{project-id}.{region}.run.app`</p>
<p>Example: `https://dev-luz-message-broker-335505349498.europe-west6.run.app`</p></td>
<td><p>Serverless connector</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>Kepler</p></td>
<td><p>`RELEASED PROD`</p></td>
<td><p>luz-thumbnail</p></td>
<td><p>-</p></td>
<td><p>-</p></td>
<td><p>-</p></td>
<td><p>`{env}-luz-thumbnail-{project-id}.{region}.run.app`</p>
<p>Example: `https://dev-luz-thumbnail-335505349498.europe-west6.run.app`</p></td>
<td><p>Serverless connector</p></td>
<td></td>
<td><p>[LUZ-152318](https://axonivy.atlassian.net/browse/LUZ-152318)</p></td>
</tr>
<tr>
<td>4</td>
<td><p>Kepler</p></td>
<td><p>`RELEASED PROD`</p></td>
<td><p>luz-storage</p></td>
<td><ul>
<li><p>luz-storage-gcs-credential-env-secret</p></li>
</ul></td>
<td><ul>
<li><p><env>-luz-storage-gcs-credential</p></li>
</ul></td>
<td><ul>
<li><p>Google Cloud Storage</p></li>
</ul></td>
<td><p>`{env}-luz-storage-{project-id}.{region}.run.app`</p>
<p>Example: `https://dev-luz-storage-335505349498.europe-west6.run.app`</p></td>
<td><p>Direct egress</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td>5</td>
<td><p>Kepler</p></td>
<td><p>`RELEASED PROD`</p></td>
<td><p>luz-storage-batch</p></td>
<td><ul>
<li><p>luz-storage-gcs-credential-env-secret</p></li>
</ul></td>
<td><ul>
<li><p><env>-luz-storage-gcs-credential</p></li>
</ul></td>
<td><ul>
<li><p>Google Cloud Storage</p></li>
</ul></td>
<td><p>`{env}-luz-storage-batch-{project-id}.{region}.run.app`</p>
<p>Example: `https://dev-luz-storage-batch-335505349498.europe-west6.run.app`</p></td>
<td><p>Direct egress</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td>6</td>
<td><p>Future</p></td>
<td><p>`TODO`</p></td>
<td><p>luz-cache</p></td>
<td><ul>
<li><p>luz-cache-env-secret</p></li>
</ul></td>
<td></td>
<td><ul>
<li><p>Google Cloud Memorystore</p></li>
</ul></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>7</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

%% ai-graph-start %%

**Related notes:**
- [[Migrate GKE to CloudRun - Gradually migrate the luz-antivirus to Cloud Run (49224646657)]]
- [[Migrate GKE to CloudRun - Gradually migrate the luz-antivirus to Cloud Run]]
- [[Luz Kubernetes Terraform]]
- [[Infrastructure]]
- [[Deploy luz-epc-redis-service on GCP]]

%% ai-graph-end %%