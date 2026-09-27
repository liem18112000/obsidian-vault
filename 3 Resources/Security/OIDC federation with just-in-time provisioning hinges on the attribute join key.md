---
ai_hash: 6369683f3fde69b0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Onboarding API Investigation Identity Provider SSO (TS)'
status: seedling
tags:
- oidc
- sso
- keycloak
- federation
- jit-provisioning
- confluence-distilled
title: OIDC federation with just-in-time provisioning hinges on the attribute join
  key
type: concept
---

# OIDC federation with just-in-time provisioning hinges on the attribute join key

When users arrive from a partner's portal, you cannot ask them to register first — they expect to land already signed in. **OIDC federation with just-in-time provisioning** handles it: the identity provider trusts the partner, and the account is created on first arrival from the attributes it receives.

The flow, with Keycloak as the broker:

1. **Register the partner as an OIDC client** in Keycloak — login URL, redirect URI, client details.
2. **Map user attributes** between the partner's representation and yours, so identities match **even when usernames differ**.
3. User authenticates at the **partner's** portal.
4. Partner redirects to Keycloak with an OIDC token carrying user information.
5. Keycloak validates it and authenticates the user. **If the user does not exist in our system, create one from the mapped attributes.** ← this is JIT provisioning.
6. Keycloak issues an access token for your API.
7. User is redirected back, and the partner can call your API on their behalf.

**Attribute mapping is the part that decides whether this works.** Step 2 is not configuration detail — it is where you choose the **join key** between two identity systems. Usernames differ, so something else must be canonical (an email, a verified national/participant id, a partner-assigned subject). Pick the wrong one and returning users get duplicate accounts, or worse, two people collapse into one.

> [!warning] JIT provisioning means the partner can create accounts in your system
> Every successful login at their end mints a user at yours. That is the intent, and it is also the risk: their onboarding quality becomes your account quality, and their compromise becomes your account creation. Decide up front **which attributes you trust them to assert** (identity, yes; entitlements or roles, usually not), and what the new account is allowed to do before any of your own verification has happened.

> [!warning] Do not put the access token in the redirect URL
> The design mentions the token *"embedded in the URL or response"*. A token in a URL leaks into browser history, `Referer` headers, server access logs, and analytics. Use the authorization-code flow and exchange the code server-side, or return the token in the response body — never as a query parameter. Same reasoning as [[Keycloak action tokens bridge an app session into a browser login]].

> [!note] Long-lived refresh tokens need a revocation story
> This design keeps a **45-day refresh token** to mint access tokens when the user jumps across. That is a long window for a credential to be valid — pair it with rotation on use and the ability to revoke centrally, or a stolen refresh token is a six-week session.

Source: [[Onboarding API - Investigation Identity Provider SSO]] (TS, Confluence).

## Related

- [[Keycloak action tokens bridge an app session into a browser login]]

%% ai-graph-start %%

**Related notes:**
- [[Exchange a partner IdP token by introspecting it, never by trusting it]]
- [[Onboarding API - Investigation Identity Provider SSO]]
- [[Keycloak action tokens bridge an app session into a browser login]]
- [[SAML documentation]]
- [[Understanding Keycloak Authorization Code flow]]

%% ai-graph-end %%