---
ai_hash: 35c02bc83b274939
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-25
entities:
- luz-jsonstore
- GKE Mongo
- port-forwards
- tenant
- luz-mongodb0N
- dev-mongodb-clusters
- app
- PRIMARY member
- db.hello().primary
- mongosh
- mongod container
- kubectl
- api-forwarder
- security
- luz-vault
- dev
- docker-compose
- CH_KLARA_JSONSTORE_RS00
- CH_KLARA_JSONSTORE_RS01
- CREATE_DB_RS
- OPS_RS
- host.docker.internal
- LUZ_SEC_HOST_PORT
- LUZ_VAULT_HOST_PORT
- Docker Desktop
- vault
- bearer token
- luz-skill-get-token
- replica set
- JAX-RS
- MessageBodyReader
- MessageBodyWriter
- applicationbson media type
- luz-mongodb0N-cluster-rs-0 pod
source: session 2026-08-25
status: seedling
tags:
- luz-jsonstore
- mongodb
- kubectl
- port-forward
- docker
title: Run local luz-jsonstore against a real tenant GKE Mongo via port-forwards
type: howto
---

# Run local luz-jsonstore against a real tenant GKE Mongo via port-forwards

To point a locally-run luz-jsonstore (docker-compose) at a real tenant's data in dev GKE:
1. Tenant's mongo cluster = luz-mongodb0N where N = (first hex char of tenantId) mod 4, namespace dev-mongodb-clusters.
2. The app connects DIRECT (no replicaSet in the URI), so forward the PRIMARY member. Find it with an unauthenticated `db.hello().primary` exec into any rs pod (mongosh in the mongod container).
3. kubectl port-forward pod/luz-mongodb0N-cluster-rs-0 27017:27017.
4. Also forward api-forwarder (security) 8080:8080 and luz-vault 8200:8200 from namespace dev.
5. In docker-compose set CH_KLARA_JSONSTORE_RS00 / RS01 / CREATE_DB_RS / OPS_RS to host.docker.internal:27017, and LUZ_SEC_HOST_PORT / LUZ_VAULT_HOST_PORT to host.docker.internal:8080 / 8200.

The container reaches host-bound port-forwards via host.docker.internal on Docker Desktop. getPassword needs vault reachable; requests need a bearer token (luz-skill-get-token issues an all-tenant token through the 8080 forward). If the replica set fails over, re-forward the new primary.

## Related

- [[Serving a custom applicationbson media type in JAX-RS via MessageBodyReader and Writer]]

%% ai-graph-start %%

**Related notes:**
- [[Local access to GKE-hosted Luz services port-forward api-forwarder + luz-vault (+ mongo pod)]]
- [[Luz services access MongoDB only through the luz_jsonstore REST API]]
- [[Build and roll out luz-jsonstore to dev (Cloud Build trigger + Deployment rollout)]]
- [[Luz local run host.docker.internal8080 must be the dev api-forwarder, not another cluster on 8080]]
- [[Reuse an existing kubectl port-forward for ad-hoc mongo scripts]]

**Relations:**
- luz-jsonstore — *runs against* — GKE Mongo
- luz-jsonstore — *uses* — port-forwards
- GKE Mongo — *stores data for* — tenant
- luz-mongodb0N — *is a* — tenant
- luz-mongodb0N — *is in namespace* — dev-mongodb-clusters
- app — *connects to* — PRIMARY member
- PRIMARY member — *identified by* — db.hello().primary
- db.hello().primary — *executed in* — mongosh
- mongosh — *runs in* — mongod container
- kubectl — *performs* — port-forwards
- port-forwards — *targets* — luz-mongodb0N-cluster-rs-0 pod
- api-forwarder — *is forwarded* — 8080
- api-forwarder — *provides* — security
- luz-vault — *is forwarded* — 8200
- api-forwarder — *is in namespace* — dev
- luz-vault — *is in namespace* — dev
- docker-compose — *sets environment variable* — CH_KLARA_JSONSTORE_RS00
- docker-compose — *sets environment variable* — CH_KLARA_JSONSTORE_RS01
- docker-compose — *sets environment variable* — CREATE_DB_RS
- docker-compose — *sets environment variable* — OPS_RS
- CH_KLARA_JSONSTORE_RS00 — *points to* — host.docker.internal
- docker-compose — *sets environment variable* — LUZ_SEC_HOST_PORT
- docker-compose — *sets environment variable* — LUZ_VAULT_HOST_PORT
- LUZ_SEC_HOST_PORT — *points to* — host.docker.internal
- LUZ_VAULT_HOST_PORT — *points to* — host.docker.internal
- luz-jsonstore — *container reaches* — host.docker.internal
- host.docker.internal — *is on* — Docker Desktop
- getPassword — *needs* — vault
- requests — *need* — bearer token
- luz-skill-get-token — *issues* — bearer token
- bearer token — *issued via* — api-forwarder
- replica set — *can* — fail over
- luz-jsonstore — *related to* — Serving a custom applicationbson media type in JAX-RS via MessageBodyReader and Writer

%% ai-graph-end %%