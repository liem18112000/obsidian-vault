---
title: "COSSA Token Exchange — Technical Analysis and Implementation"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49354702852/COSSA+Token+Exchange+Technical+Analysis+and+Implementation
space: "TS"
topic: programming
relevance: 0.857
depth: 3
updated: 2026-05-04
attachments: 2
tags:
  - confluence
  - programming
  - space/ts
---

# COSSA Token Exchange — Technical Analysis and Implementation

> [!info] Imported from Confluence
> Space **TS** · updated 2026-05-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49354702852/COSSA+Token+Exchange+Technical+Analysis+and+Implementation)
> Relevance 0.857 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="79d6613d-5514-471b-b26a-c9b1ed0312d8" macro-name="toc">

</div>

# Overview

This document consolidates the analysis, specification, open questions, and the proposed implementation concept for exchanging COSSA access tokens from Swiss Post into ePost tokens consumable by our Public API. The end-user goal: when authenticated on post.ch (Swiss Post), the user should seamlessly access their ePost digital mailbox without re-authentication.

<div hasbody="true" macro-id="b2084e6b-3919-48fb-86bf-b24db2a7afe1" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Primary reference: <a href="https://axonivy.atlassian.net/browse/LUZ-147804" class="external-link" rel="nofollow">Jira Story LUZ-147804 “Technical Analysis and Implementation Concept for COSSA Token Exchange” — https://axonivy.atlassian.net/browse/LUZ-147804</a>

</div>

</div>

## Goal

- When a user logged in to <a href="http://post.ch/" class="external-link" rel="nofollow"><u>http://post.ch</u></a> , he/she can automatically log in to his/her digital mailbox in ePost.

# Background

## Standards and Endpoints

- Token Introspection: introspection is performed with Basic Authentication against Swiss Post.

- OpenID Connect Discovery (int): <a href="https://apiint.post.ch/.well-known/openid-configuration" class="external-link" rel="nofollow">https://apiint.post.ch/.well-known/openid-configuration</a>

- Introspection sandbox endpoint: <a href="https://apiint.post.ch/OAuth/introspect" class="external-link" rel="nofollow">https://apiint.post.ch/OAuth/introspect</a>

- Introspection production endpoint: <a href="https://api.post.ch/OAuth/introspect" class="external-link" rel="nofollow">https://api.post.ch/OAuth/introspect</a>

- Userinfo endpoint: <a href="https://apiint.post.ch/openid/userInfo" class="external-link" rel="nofollow">https://apiint.post.ch/userinfo/</a>

## Example Introspection Payload (Integration)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3cd248cf-6c3e-4c82-98ed-31c1423990e6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "active": true,
  "scope": "offline_access profile openid no_consent_required email KVM_CUSTOMER_ACTIVITY",
  "client_id": "5990c8b9d846a095a795a1bac2e7f1db",
  "sub": "10428382",
  "exp": 1770818656,
  "iat": 1770818356,
  "nbf": 1770818356,
  "aud": [],
  "iss": "https://apiint.post.ch",
  "metadata": {
    "openid.claims.requested": "{...}",
    "swissid.sub": "7shJpTdR/JzNevajEU1OVTfPx5mNYhvITrMxZpDNgNk="
  }
}
```

</div>

</div>

# End-to-End Flow

The following sequence diagram models the intended interaction between the Swiss Post client, our Public API, our Keycloak realm, and Swiss Post Introspection/UserInfo services.


![[49354702852-cossa-token-exchange-sequence-diagram-2026-05-04-103727.png]]



## High-level Steps

1.  Client calls our Public API: `POST /core/latest/token` with `grant_type`=`cossa_token` and `cossa_access_token`.

2.  Public API call to JWT Service, then forwards to Keycloak custom token-exchange realm endpoint with Authorization: Bearer `<cossa_token>` and `client_id`=`<keycloak-public-api-client-id>`.

3.  If inactive or null → 401 invalid_grant.

4.  Keycloak calls Swiss Post UserInfo (when necessary) using the same access token to retrieve claims such as email.

5.  First, by federated identity link `swissid.sub` (preferred),

6.  Fallback by `KLP email` (from UserInfo) if federated link not present.

7.  User must exist and be enabled (else 423 user_disabled or 404 unknown_user).

8.  Tenant constraints: only individual tenants (company_id must not indicate business; missing or non-individual → 403 no_individual_tenant).

9.  On success, Keycloak issues tokens (`access_token`, `refresh_token`, `expires_in`, `refresh_expires_in`, `token_type`) to Public API; Public API responds to the client.

# Detailed Design

## Public API Contract

- Endpoint: POST `/core/latest/token`

- `grant_type=cossa_token`

- `cossa_access_token=<opaque_token_value>`

- Reject 400 if `cossa_access_token` is missing/blank.

- Call Keycloak `/realms/{realm}/token-exchange` with Authorization: Bearer `<COSSA token>` and `client_id` for our Public API.

- Surface Keycloak error mappings to the client.

- 200 JSON body: `access_token`, `refresh_token`, `expires_in`, `token_type`.

## Keycloak Token-Exchange Provider

Implement a Keycloak custom *Identity Provider/Protocol Mapper* or SPI that:

- Detects non-JWT opaque bearer tokens and routes to COSSA introspection.

- BasicAuth using Swiss Post-provided credentials.

- POST form-encoded: `token=<cossa_access_token>`.

- `active` from the introspection result must be true.

- Extract identity keys: `metadata.swissid.sub` (federated ID), `sub` (Swiss Post subject), `iss`, `client_id` (informational if behind API-M).

- GET `userinfo_endpoint` with Bearer token.

- Expect `email` when consent/scope granted; note: email is from the KLP account.

- First: find the user by the federated identity link on `swissid.sub`. If missing and an email exists, fallback to email match.

- If a user is found by email, but the link is missing and `swissid.sub` present, create or update the federated identity link.

- Persist `swiss_post_sub` (or equivalent attribute) for traceability.

- Reject if user not found (404) or disabled (423).

- **Only individual tenants** are supported at this time. Reject with 403 if `company_id` indicates business or `tenant_id`/`company_id` is missing/invalid.

- Issue access/refresh tokens with realm roles and claims (`tenant_id`, `company_id`) required by ePost.

- Include `refresh_token` to support caching and rate reduction, per Swiss Post recommendation.

## Errors and Mappings

- 400 Bad Request: missing `cossa_access_token`.

- 401 invalid_client: when Keycloak client authentication to Swiss Post fails (introspection BasicAuth) or invalid `client_id`/secret to Keycloak endpoint.

- 401 invalid_grant: token inactive, introspection `active=false`, or insufficient identity data (e.g., both email and swissid.sub missing).

- 404 unknown_user: no matching user by swissid.sub or email fallback.

- 423 user_disabled: matched user is disabled.

- 403 no_individual_tenant: company_id indicates business or tenant constraints not satisfied.

## API and Keycloak Contract Snippets

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e4b49789-b5a0-416e-be4f-5c05dc22e79a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Introspection
curl -X POST -u "${INTROSPECTION_USER}:${INTROSPECTION_PWD}" \
  "https://apiint.post.ch/OAuth/introspect" \
  --data-urlencode "token=${ACCESS_TOKEN}"
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="254b87fe-de99-4302-91bf-f56fff6ff2f2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Public API request

curl --location 'http://localhost:8080/core/latest/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=cossa_token' \
--data-urlencode 'cossa_access_token={cossa_token}'
```

</div>

</div>

# Open Questions and Decisions

- Support scope: individual tenants only (reject business tenants).
- User resolution precedence: federated identity via `swissid.sub`, then `KLP email` fallback.

<!-- -->

- Common issues: What should we do if a user has multiple individual tenants?

# Testing Matrix

<div>

|  |  |  |
|----|----|----|
| Test Case | Input/Preconditions | Expected Result |
| Missing token | grant_type=cossa_token, no cossa_access_token | 400 Bad Request |
| Inactive token | Introspection active=false | 401 invalid_grant |
| No identity data | No swissid.sub and no email from UserInfo | 401 invalid_grant |
| User not found | Valid token; no user by federated link or email | 404 unknown_user |
| User disabled | Valid user resolved but disabled | 423 user_disabled |
| Business tenant | company_id indicates non-individual | 403 no_individual_tenant |
| Happy path | Active token, valid scopes, swissid.sub present; user enabled | 200 with access_token, refresh_token, expires_in, token_type |

</div>

# References

- Jira Story: <a href="https://axonivy.atlassian.net/browse/LUZ-147804" class="external-link" rel="nofollow">LUZ-147804 — Technical Analysis and Implementation Concept for COSSA Token Exchange — https://axonivy.atlassian.net/browse/LUZ-147804</a>

- Swiss Post OIDC Discovery (INT): <a href="https://apiint.post.ch/.well-known/openid-configuration" class="external-link" rel="nofollow">https://apiint.post.ch/.well-known/openid-configuration</a>
