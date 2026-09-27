---
ai_hash: b288b6b778727d77
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-13
entities: []
source: session 2026-09-13
status: seedling
tags:
- oauth2-proxy
- sso
- caddy
- 502
- gotcha
- leo-customer360
title: oauth2-proxy cookie_secret must be 16/24/32 bytes; openssl rand -base64 32
  (44 chars) crash-loops it
type: lesson
---

# oauth2-proxy cookie_secret must be 16/24/32 bytes; openssl rand -base64 32 (44 chars) crash-loops it

oauth2-proxy requires `--cookie-secret` (env `OAUTH2_PROXY_COOKIE_SECRET`) to decode/measure to **exactly 16, 24, or 32 bytes** (AES-128/192/256). Generating it with `openssl rand -base64 32` yields a **43-44 character** string, which oauth2-proxy rejects at startup with `cookie_secret must be 16, 24, or 32 bytes to create an AES cipher, but is 44 bytes` and **crash-loops forever** (exit 1, Restarting).

**Symptom seen (leo-customer360 UAT):** `https://<host>/jaeger` returned **502**. Jaeger itself was healthy; the 502 was because the oauth2-proxy gate in front of it (`c360-oauth2-jaeger`, 613 restarts) was down, so Caddy could not reach its upstream. The same shared secret gates Netdata + pgAdmin, so all oauth2 gates break together.

**Diagnosis path:** Caddy 502 -> `docker ps -a` shows the *gate* container Restarting, not the app -> `docker logs` on the gate shows the config error. A crash-looping SSO sidecar, not the protected app, is the usual cause of a 502 on an SSO-gated ops tool.

**Fix:** generate a valid-length secret, e.g. `openssl rand -hex 16` (32 hex chars = 32 bytes) or `openssl rand -base64 24 | tr -dc A-Za-z0-9 | head -c 32`; delete the bad value so the deploy regenerates it. Root cause here was `deploy-monitoring.sh` line 190 hardcoding `openssl rand -base64 32`.

## Related
[[docs-search UAT latency root cause: unapplied 8001 secgroup ingress (api to docs box)]]

## Related

- [[docs-search UAT latency root cause: unapplied 8001 secgroup ingress (api to docs box)]]

%% ai-graph-start %%

**Related notes:**
- [[Monitoring SSO-gate adding a dashboard needs a Keycloak redirect_uri re-sync]]
- [[Serve a no-auth UI over TLS+SSO behind Caddy at a subpath (oauth2-proxy proxy-prefix)]]
- [[Jaeger on a small box use in-memory bounded storage, not badger, to avoid OOM 502]]
- [[docs-search UAT latency root cause unapplied 8001 secgroup ingress (api to docs box)]]
- [[Shared OIDC client skip-if-secret-exists guard drops new redirect URIs (Invalid redirect_uri)]]

%% ai-graph-end %%