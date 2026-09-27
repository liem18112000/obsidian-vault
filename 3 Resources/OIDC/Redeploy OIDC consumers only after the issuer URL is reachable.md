---
ai_hash: 4bdaf69836948dc1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 beta.leocdp.com cutover, 2026-08
status: seedling
tags:
- oidc
- keycloak
- oauth2-proxy
- deployment
- cutover
title: Redeploy OIDC consumers only after the issuer URL is reachable
type: lesson
---

# Redeploy OIDC consumers only after the issuer URL is reachable

When you change an OIDC issuer/public URL, redeploy the **relying parties** (oauth2-proxy, an API doing token introspection/JWKS) only AFTER that issuer is actually reachable over the new URL — because they fetch `<issuer>/.well-known/openid-configuration` at **startup**. oauth2-proxy in particular crash-loops if discovery fails, so bringing it up before the front door (proxy/LB/cert) is live breaks the whole cutover.

**Safe cutover order (domain/TLS switch):** (1) reconfigure the IdP hostname; (2) stand up the proxy/LB that serves it; (3) `curl <issuer>/.well-known/openid-configuration` to confirm; (4) THEN redeploy the relying parties. The IdP (e.g. Keycloak) itself can restart first — it only stamps the new hostname into generated URLs, it does not need the public URL reachable to boot.

Related: [[Keycloak behind a TLS-terminating proxy needs proxy headers and hostname-strict off]]

## Related

- [[Keycloak behind a TLS-terminating proxy needs proxy headers and hostname-strict off]]

%% ai-graph-start %%

**Related notes:**
- [[Keycloak behind a TLS-terminating proxy needs proxy headers and hostname-strict off]]
- [[Keycloak 26 OIDC issuer path comes from KC_HOSTNAME, not KC_HTTP_RELATIVE_PATH]]
- [[Shared OIDC client skip-if-secret-exists guard drops new redirect URIs (Invalid redirect_uri)]]
- [[KC_HTTP_RELATIVE_PATH moves Keycloak health endpoints under the prefix too]]
- [[Keep Keycloak realm roles in sync with app authz constants, and ensure the bootstrap step is in the CD services list]]

%% ai-graph-end %%