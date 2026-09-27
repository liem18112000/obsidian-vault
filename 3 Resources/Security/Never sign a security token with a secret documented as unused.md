---
title: "Never sign a security token with a secret documented as unused"
created: 2026-09-19
type: lesson
status: seedling
source: "leo-customer360 SCRUM-102 code review 2026-09-19"
tags: [security, auth, oauth, csrf, secrets, gotcha]
---

# Never sign a security token with a secret documented as unused

A security-critical signature must use a secret that is **required and validated (non-default) in every deployment mode where the protected path is reachable**. Never sign it with a secret whose own config comment says it is inert in that mode — operators trust the comment and leave the secret at its public default, making the signature forgeable.

## Why it bites
The comment ("never used when SSO_LOGIN=true") is a *promise* that turns into an *attack surface*: it tells ops not to bother setting the secret, so the default ships to prod. If any reachable code path still signs/verifies with that secret, the whole trust boundary rests on a value everyone can read from the source.

## Tell-tale shape
- A public / auth-exempt endpoint whose only tenant/identity binding is a signed token.
- That token signed with a secret shared from another feature that is "off" in this mode.
- The secret has a hardcoded default (`dev-insecure-...`) and no prod-required validation.

## Real instance
leo-customer360: the public `zalo-redirect` OAuth callback (in `EXEMPT_PATHS`) binds to a tenant solely via a `state` JWT signed with `dev_jwt_secret` — defaulted to `dev-insecure-secret-change-me-please-32b` and documented as unused under SSO. Anyone knowing the default forges a state binding their OA `code` to a victim tenant → cross-tenant OAuth token injection.

## Fix pattern
Give each security-critical signer its *own* secret, fail startup if it is unset/default in prod, independent of any "mode" flag.

## Related

- [[CREATE TABLE IF NOT EXISTS cannot express a rename]]
