---
ai_hash: 20dead07a34dd450
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 deploy-sso.sh, 2026-08
status: seedling
tags:
- keycloak
- health-check
- relative-path
- deployment
- gotcha
title: KC_HTTP_RELATIVE_PATH moves Keycloak health endpoints under the prefix too
type: gotcha
---

# KC_HTTP_RELATIVE_PATH moves Keycloak health endpoints under the prefix too

Setting `KC_HTTP_RELATIVE_PATH=/auth` in Keycloak (26) relocates NOT ONLY the main HTTP endpoints but ALSO the management-interface health/metrics endpoints on port :9000. So after setting it, `http://127.0.0.1:9000/health/ready` returns 404 and the real probe is `http://127.0.0.1:9000/auth/health/ready` (200). Likewise `:8080/realms/master` -> 404, `:8080/auth/realms/master` -> 200.

**Gotcha:** a deploy/readiness script that hardcodes `:9000/health/ready` will wrongly report "not ready within timeout" even though Keycloak started fine. Build the probe URL as `:9000${RELPATH}/health/ready`.

Unrelated-but-nearby noise: Keycloak/vert.x logs a bare `ERROR ... Unhandled exception in router` (no stack trace) for connection-level oddities (client disconnect, partial/non-HTTP request, a plain-HTTP hit on an endpoint while KC_HOSTNAME is https and no X-Forwarded-* is present). No stack + valid requests still 200 = benign; ignore it.

Related: [[Keycloak behind a TLS-terminating proxy needs proxy headers and hostname-strict off]]

## Related

- [[Keycloak behind a TLS-terminating proxy needs proxy headers and hostname-strict off]]

%% ai-graph-start %%

**Related notes:**
- [[Keycloak 26 OIDC issuer path comes from KC_HOSTNAME, not KC_HTTP_RELATIVE_PATH]]
- [[Keycloak behind a TLS-terminating proxy needs proxy headers and hostname-strict off]]
- [[KLARA web availability probe must follow redirects and inspect the final URL]]
- [[Deploying Keycloak 26 as a container health port 9000, bootstrap admin, start vs start-dev]]
- [[Redeploy OIDC consumers only after the issuer URL is reachable]]

%% ai-graph-end %%