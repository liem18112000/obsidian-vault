---
ai_hash: 0bd376ccca08ef97
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 SSO wiring 2026-08-19
status: seedling
tags:
- keycloak
- oidc
- introspection
- audience
- gotcha
- sso
title: Keycloak 24+ token introspection requires the client in the token audience
type: lesson
---

# Keycloak 24+ token introspection requires the client in the token audience

Keycloak 24+ hardened the token introspection endpoint: it returns {"active":false}
for a perfectly valid, unexpired token UNLESS the client authenticating the introspect
call is listed in the token's `aud` (audience) claim. The server log shows the real reason:
`INTROSPECT_TOKEN_ERROR ... reason="Client X is not in the token audience"`.

This is silent and misleading -- the token passes userinfo (HTTP 200) and is genuinely
active, but a resource server that validates via introspection rejects every request.

Fix: add an **audience protocol mapper** to the client (or a shared client scope):
protocolMapper `oidc-audience-mapper`, config `included.client.audience=<that-client-id>`,
`access.token.claim=true`, `introspection.token.claim=true`. After that the token's aud
includes the client and introspection returns active:true.

Related debugging facts from the same session (Keycloak 26, direct-grant testing):
- `curl -d "password=..."` does NOT url-encode; a password with `%`/`#` corrupts the form
  body -> use `--data-urlencode`.
- "Account is not fully set up" (invalid_grant) = the user is missing required profile
  fields (email/firstName/lastName) or has pending requiredActions.
- userinfo returns 403 "Missing openid scope" if the token was minted without scope=openid.

Discovered wiring leo-customer360 customer360-api to Keycloak. Related: [[FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set]]

## Related

- [[FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set]]

%% ai-graph-start %%

**Related notes:**
- [[LEO CDP tenant isolation fails closed and needs a Keycloak tenant_id claim mapper]]
- [[Keep Keycloak realm roles in sync with app authz constants, and ensure the bootstrap step is in the CD services list]]
- [[Creating a Keycloak realm role does not grant it — you must assign it and match the name the app authorizes on]]
- [[customer360-api has two auth modes dev local-JWT vs Keycloak SSO]]
- [[Shared OIDC client skip-if-secret-exists guard drops new redirect URIs (Invalid redirect_uri)]]

%% ai-graph-end %%