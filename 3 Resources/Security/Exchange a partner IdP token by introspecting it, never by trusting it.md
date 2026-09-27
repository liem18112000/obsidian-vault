---
ai_hash: a93a1367c8ecbfef
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: COSSA Token Exchange Technical Analysis (TS)'
status: seedling
tags:
- oauth
- oidc
- token-exchange
- introspection
- keycloak
- sso
- confluence-distilled
title: Exchange a partner IdP token by introspecting it, never by trusting it
type: concept
---

# Exchange a partner IdP token by introspecting it, never by trusting it

To let a user authenticated at a partner identity provider reach your API without logging in again, you do **not** trust their token directly. You exchange it: accept the foreign access token at a dedicated grant, verify it with the issuer, then mint your own.

The shape, from a Swiss Post (COSSA) → ePost integration:

1. Client calls your API with a custom grant:
   ```
   POST /core/latest/token
   grant_type=cossa_token&cossa_access_token=<foreign token>
   ```
2. Your token service forwards to a Keycloak **token-exchange** realm endpoint.
3. Keycloak **introspects** the foreign token against the partner (`POST /OAuth/introspect`, Basic auth as a registered client).
4. If the response says `active: false` — or nothing comes back — return **401 `invalid_grant`**.
5. If claims are missing (e.g. email), call the partner's **UserInfo** endpoint with the same access token.
6. Mint and return *your* token.

A representative introspection response:

```json
{
  "active": true,
  "scope": "offline_access profile openid email KVM_CUSTOMER_ACTIVITY",
  "client_id": "5990c8b9…", "sub": "10428382",
  "exp": 1770818656, "iat": 1770818356, "nbf": 1770818356,
  "iss": "https://apiint.post.ch",
  "metadata": { "swissid.sub": "7shJpTdR/JzNevajEU1OVTfPx5mNYhvITrMxZpDNgNk=" }
}
```

**Why introspection rather than local JWT validation.** A partner's opaque token has no signature you can check, and even for a JWT, local validation cannot see **revocation**. `active: true` is a live answer from the issuer at this instant — which is the point of accepting someone else's credential. The cost is a synchronous call to the partner on every exchange, so cache the *result* briefly if at all, never past `exp`.

**Two integration details worth stealing:**

- **Discovery over hardcoded URLs.** `/.well-known/openid-configuration` gives you introspection, userinfo, and JWKS endpoints per environment — so the int/prod split (`apiint.post.ch` vs `api.post.ch`) is one config value, not four.
- **Introspection and UserInfo are different questions.** Introspection answers *"is this token valid and what scopes does it carry?"*; UserInfo answers *"who is the human?"*. You often need both, and the claim you want (email) is frequently only in the second.

> [!warning] Decide which subject identifier is your join key
> This payload carries **two**: `sub` (the partner's local user id) and `metadata.swissid.sub` (a federated pseudonymous id). They are not interchangeable, and picking the wrong one means account linking breaks the day the partner changes their internal ids — or worse, two users collide. Pin this down before the first user is linked; it is very expensive to change afterwards.

Source: [[COSSA Token Exchange — Technical Analysis and Implementation]] (TS, Confluence).

## Related

- [[Authorization has four named parts PAP, PDP, PEP, PIP]]

%% ai-graph-start %%

**Related notes:**
- [[COSSA Token Exchange — Technical Analysis and Implementation]]
- [[OIDC federation with just-in-time provisioning hinges on the attribute join key]]
- [[Onboarding API - Investigation Identity Provider SSO]]
- [[SAML documentation]]
- [[Understanding Keycloak Authorization Code flow]]

%% ai-graph-end %%