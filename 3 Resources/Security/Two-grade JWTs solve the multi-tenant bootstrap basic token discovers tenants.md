---
title: "Two-grade JWTs solve the multi-tenant bootstrap: basic token discovers tenants"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Token JWT Security (LUZ)"
tags: [jwt, multi-tenancy, authentication, token-service, jwks, confluence-distilled]
---

# Two-grade JWTs solve the multi-tenant bootstrap: basic token discovers tenants

In a multi-tenant system there is a bootstrap problem: the token should carry the tenant, but at login the caller does not yet know which tenants they have. The resolution is **two token grades**, acquired in sequence.

**1. Basic token** — subject only (the username). Obtained with Basic Auth:

```
POST /luzsec/api/tokens        (Basic Auth: username + password)
```

It is accepted by resources annotated `@AccessibleWithoutTenant` — endpoints that need to know *who* you are but not *which tenant* you are acting for.

**2. Discover the tenants** using that basic token:

```
GET /luztenant/api/{username}/tenants     Authorization: Bearer <basic token>
```

**3. Full token** — carries the tenant claims (person-tenant-id, company-tenant-id) for one chosen tenant:

```
POST /luzsec/api/{company-tenant-id}/tokens    (Basic Auth)
```

Required for any resource that touches tenant-scoped data.

**Why grade the tokens instead of putting every tenant in one.** A token listing all of a user's tenants would grant access to all of them on every request, so any endpoint would have to re-derive intent from a parameter — and a stolen token would be scoped to everything the user can reach. Naming the tenant *in the token* makes the scope explicit and auditable: the credential says what it is for.

**Signing and verification.** Tokens are JSON, Base64-encoded, and **signed with a private key**. The public key is published so any service can verify without calling the issuer:

```
GET /luzsec/api/public-keys/active      (no auth)
```

That endpoint being unauthenticated is correct — a public key is public by definition, and requiring auth to fetch it would make every verifier a client of the issuer, reintroducing the coupling that asymmetric signing exists to remove.

> [!warning] Verify the signature, do not just decode it
> A JWT is **encoded, not encrypted** — anyone can Base64-decode it and read every claim. Two consequences: never put a secret in a claim, and never trust a claim without checking the signature against the published key. Libraries that "decode" a JWT by default are the usual source of this bug.

> [!tip] Cache the public key, but honour rotation
> `/public-keys/active` implies keys rotate. Fetching it on every request is a synchronous dependency on the issuer; caching forever breaks on rotation. Cache with a TTL and re-fetch on an unknown key id — which is the problem a JWKS endpoint with `kid` headers solves properly.

Related: [[Exchange a partner IdP token by introspecting it, never by trusting it]] — the opposite case, where local verification is not enough.

Source: [[Token JWT Security]] (LUZ, Confluence).

## Related

- [[Exchange a partner IdP token by introspecting it, never by trusting it]]
