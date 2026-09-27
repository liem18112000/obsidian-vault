---
ai_hash: daa52a5726751256
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-30
entities: []
source: session 2026-08-30 proxy script
status: seedling
tags:
- gcp
- cloud-sql
- cloud-sql-proxy
- pgadmin
- gotcha
title: cloud-sql-proxy binaries ship in a GCS bucket, not GitHub release assets
type: gotcha
---

# cloud-sql-proxy binaries ship in a GCS bucket, not GitHub release assets

The Cloud SQL Auth Proxy v2 (`cloud-sql-proxy`) binaries are NOT attached as GitHub release assets — the repo `GoogleCloudPlatform/cloud-sql-proxy` publishes releases with notes but no downloadable binaries. So `https://github.com/.../releases/latest/download/cloud-sql-proxy.x64.exe` returns **404**.

The binaries live in a Google **storage bucket**, versioned by tag:

```
https://storage.googleapis.com/cloud-sql-connectors/cloud-sql-proxy/<TAG>/<asset>
# Windows amd64 asset name: cloud-sql-proxy.x64.exe   (linux: cloud-sql-proxy.linux.amd64, etc.)
```

To always get the newest: resolve the latest tag from the GitHub API (`.../releases/latest` → `tag_name`, e.g. `v2.25.4`) and interpolate it into the bucket URL. Verify with `curl -I` (expect 200) before scripting. The proxy authenticates with your gcloud ADC and needs `roles/cloudsql.client`; it exposes a local TCP port so any client (pgAdmin/DBeaver/psql) can reach an instance that has no public DB access. Related: [[Cloud Run to Cloud SQL via Auth-proxy unix socket with asyncpg]].

## Related

- [[Cloud Run to Cloud SQL via Auth-proxy unix socket with asyncpg]]

%% ai-graph-start %%

**Related notes:**
- [[Cloud SQL Python Connector async use create_async_connector inside the loop]]
- [[Cloud Run to Cloud SQL via Auth-proxy unix socket with asyncpg]]
- [[gcloud components install fetches the host's own platform binary, not a chosen target]]
- [[Local Cloud SQL admin without psql use the Python connector + asyncpg]]
- [[Cloud SQL Auth Proxy needs roles-cloudsql.client on the connecting identity or it 403s NOT_AUTHORIZED]]

%% ai-graph-end %%