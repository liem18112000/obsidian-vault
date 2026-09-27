---
ai_hash: 8c69a7b7553ee32f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities: []
source: session 2026-09-03
status: seedling
tags:
- gcp
- cloud-run
- cloud-sql
- sidecar
- gotcha
title: Cloud Run's managed /cloudsql socket does not reach sidecar containers
type: lesson
---

# Cloud Run's managed /cloudsql socket does not reach sidecar containers

In a Cloud Run multi-container (sidecar) service, the automatic Cloud SQL connection — the `/cloudsql/<INSTANCE_CONNECTION_NAME>` unix socket the platform mounts when you attach a Cloud SQL instance — is only provisioned inside the **ingress** container, not in sidecar containers. A sidecar that opens that socket (e.g. asyncpg with `host=/cloudsql/<conn>`) dies with `FileNotFoundError [Errno 2]` because the socket was never created in its filesystem.

The trap: the service-level `cloud_sql_instance` volume and the sidecar's `volumeMounts:[/cloudsql]` all look correct in Terraform and in the Cloud Run v2 API, so nothing appears misconfigured — yet the managed proxy simply doesn't create the socket for the sidecar. A clean fresh revision / `terraform -replace` does not fix it either.

Fix: if your DB-using process runs in the sidecar (not the ingress container), stop relying on the mounted socket and connect **socketlessly** — use the Cloud SQL Python Connector, which dials the instance over the container's normal egress with IAM + TLS (needs `roles/cloudsql.client` + the Cloud SQL Admin API enabled) and therefore works from any container regardless of which one is ingress. Alternatives: run the Cloud SQL Auth Proxy as an explicit TCP sidecar and connect to 127.0.0.1, or make the DB container the ingress one.

## Related

- [[gcloud builds submit --suppress-logs still exits non-zero on the log-streaming permission error]]

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run mounts the Cloud SQL cloudsql socket only into the ingress container, not sidecars]]
- [[Cloud Run to Cloud SQL via Auth-proxy unix socket with asyncpg]]
- [[gcloud builds submit --suppress-logs still exits non-zero on the log-streaming permission error]]
- [[Cloud SQL Auth Proxy needs roles-cloudsql.client on the connecting identity or it 403s NOT_AUTHORIZED]]
- [[Cloud Run v2 multi-container sidecar in Terraform]]

%% ai-graph-end %%