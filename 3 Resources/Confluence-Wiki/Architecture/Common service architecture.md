---
ai_hash: 98203b32b34de057
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 16
depth: 2.73
entities: []
relevance: 0.806
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47841017916/Common+service+architecture
space: FUT
status: reference
tags:
- confluence
- architecture
- space/fut
title: Common service architecture
topic: architecture
type: source
updated: 2024-07-02
---

# Common service architecture

> [!info] Imported from Confluence
> Space **FUT** · updated 2024-07-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47841017916/Common+service+architecture)
> Relevance 0.806 · topic `architecture`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6" macro-id="b527f540-0b0b-4a16-b997-73a5ba0aa029" macro-name="toc" numberedoutline="false" structure="list">

</div>

# Overview

Google cloud run is chose to deploy common service that can be orchestrated.


![[47841017916-image-20240607-041943.png]]



<div>

|              |                        |                  |
|--------------|------------------------|------------------|
| **Language** | **Application server** | **Startup time** |
| Java         | Quarkus native         | ~500ms           |
| Typescript   | NodeJs                 | ~100ms           |

</div>

# Idempotency

In Common Service Architecture, one event can be processed multiple times, or concurrently or even race condition.

The first step of idempotency topic is to solve race condition situation. We want to guarantee one event can only be processed once at a time.


![[47841017916-Common service architecture - Event Flowchart.png]]



## Event entity

Single table: **idempotency**

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Column name</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>idempotency_id (PK, uuid)<br />
(4f3fb963-85bf-4309-bca2-defbf4ae6164)</p></td>
<td><p>the uuid of an event. We want to control the unique constraint for both cases PubSub and HttpCall.</p></td>
<td><p>The client must assign an uuid for each event, to guarantee the unique constraint.</p>
<p>e.g when publishing a message, the client must assign an uuid for that message.</p></td>
</tr>
<tr>
<td><p>expiry_time (timestamp, not null)<br />
2024-06-05 16:13:06.304</p></td>
<td><p>the time that an event will be expired</p></td>
<td><p>calculated by:<br />
request received time + request timeout</p>
<p>Each cloud run for each feature will have different timeout</p></td>
</tr>
</tbody>
</table>

</div>


![[47841017916-image-20240619-045047.png]]



# Database options

To handle those steps, the requirement for database is required. Beside that, the operation is almost “create” with the large traffic might happen concurrently as work with Cloud Run. The database must to have all the advantages of high availability, high scalability, high performance especially write operation and follow ACID complaint to ensures data consistence and data integrity in transaction processing or in race condition or even in system failure.

As SQL supports strong ACID compare to noSQL, we only consider SQL database candidates:

- CockroachDB

  - Good

    - postgres compatibility

    - fully acid

    - distributed database architecture

    - high throughput in high traffic (write operation)

    - high scalability, high availability

    - support cleanup data with custom TTL for each row <a href="https://www.cockroachlabs.com/docs/stable/row-level-ttl" class="external-link" rel="nofollow">Batch Delete Expired Data with Row-Level TTL</a>

    - support unlimited datasets

    - geo-partitioning, multi-region

  - Trade-off

    - cost at deployment (multi nodes in separate cluster)

  - Support fully managed CockroachDB Dedicated (cockroach labs) on gcp <a href="https://console.cloud.google.com/marketplace/product/cockroachdb-public/cockroachdb?project=klara-nonprod" class="external-link" rel="nofollow">pricing</a> (very high cost)

→ very strong candidate, can fit our needs but cost for deployment

- PostgreSQL DB

  - Good

    - fully acid

    - high throughput in small to medium traffic (writes)

    - support large datasets

    - high availability

  - Limit

    - no auto cleanup job is available

  - Support fully managed product on gcp

    - CloudSQL <a href="https://cloud.google.com/sql/?utm_source=google&amp;utm_medium=cpc&amp;utm_campaign=japac-VN-all-en-dr-BKWS-all-super-trial-EXA-dr-1605216&amp;utm_content=text-ad-none-none-DEV_c-CRE_658272193331-ADGP_Hybrid+%7C+BKWS+-+BRO+%7C+Txt+-Databases-Cloud+SQL-googe+cloud+sql-main-KWID_43700076375726804-aud-970366092687:kwd-297302065178&amp;userloc_1028581-network_g&amp;utm_term=KW_google+cloud+sql&amp;gad_source=1&amp;gclid=Cj0KCQjw3tCyBhDBARIsAEY0XNlAFCevKr2PxV682TgP_g7-YIhXMeK7PPTMJVT09e2I2BXoiz2fYV0aAgWWEALw_wcB&amp;gclsrc=aw.ds&amp;hl=en#pricing" class="external-link" rel="nofollow">pricing</a>

    - Alloydb <a href="https://cloud.google.com/alloydb/pricing" class="external-link" rel="nofollow">pricing</a> (higher performance → more cost effective compare to cloudsql with the same workload)

→ fit our needs with significant power, support fully-managed on infrastructure on GCP with affordable cost

<div>

|  |  |  |
|----|----|----|
|   | **Self-managed** | **Fully-managed** |
| use case | consistently high-traffic | unpredictable traffic pattern |
| deployment/scaling/resources | self control/config | no control |
| maintenance | yes | no |
| pricing | low | high |

</div>

**Conclusion**

PostgreSQL DB seems to be good for our needs to store event state during the event handling for idempotency, also supports deployment fully-managed to reduce manual works. **But with simple database structure and simple use case as current we start first with CloudSQL**.


![[47841017916-image-20240609-163643.png]]



# Cleanup expired idempotency records


![[47841017916-image-20240614-082635.png]]

![[47841017916-image-20240619-064011.png]]



<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Kubernetes cronjob (<span>chose</span>)</strong></p></th>
<th><p><strong>Cloud scheduler + Cloud run job</strong></p></th>
</tr>
&#10;<tr>
<td><p>Availability</p></td>
<td><p>share resources in node, sometimes cannot be available because of out of resource</p></td>
<td><p>google ensures</p></td>
</tr>
<tr>
<td><p>Effort to implement</p></td>
<td><p>less</p></td>
<td><p>new HTTP api to cloud scheduler can trigger</p></td>
</tr>
<tr>
<td><p>Cost optimize</p></td>
<td><p>share with existing resources</p>
<p>→ no extra cost</p></td>
<td><p>Cloud scheduler: <a href="https://cloud.google.com/scheduler/pricing" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/scheduler/pricing</a></p>
<ul>
<li><p>3 jobs free per month</p></li>
<li><p>0.1$ job/month</p></li>
</ul>
<p>Cloud run job <a href="https://cloud.google.com/run/pricing" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/run/pricing</a></p>

![[47841017916-Screenshot 2024-06-17 114958.png]]


<p>→ small task can take advantage of free tier provided by google with less cost</p></td>
</tr>
</tbody>
</table>

</div>


![[47841017916-image-20240702-064554.png]]



Diagrams 

![[47841017916-gke-workload-identity.drawio (1).png]]



Refer <a href="https://cloud.google.com/kubernetes-engine/docs/concepts/workload-identity" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/kubernetes-engine/docs/concepts/workload-identity</a>

# Connection to GKE and CloudSQL

[Cloud Run, GKE and Cloud SQL together](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47855697940/Cloud+Run+GKE+and+Cloud+SQL+together)


![[47841017916-common-service-networking.png]]



# Sensitive environment variables

## Secret Manager


![[47841017916-Encrypt Key.png]]



## Secret using flow

Sensitive information→ encrypt sensitive info using SOPS and keys in Cloud KMS → commit Bitbucket → decrypt sensitive info using SOPS and keys in Cloud KMS → Create Secrets.

<span class="inline-comment-marker" ref="a549cca6-bb46-4ca6-9bad-e6fd683c4f1e">

![[47841017916-secret flow.png]]

</span>

Use secret in Cloud Run Service

- Environment variable refer to secret

%% ai-graph-start %%

**Related notes:**
- [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]
- [[How to implement a service]]
- [[Aggregation Database Table Design]]
- [[Recipe Best practices implementing scalable distributed applications on cloud]]
- [[Confluence-Distillation]]

%% ai-graph-end %%