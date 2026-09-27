---
title: "Token age checks need clock-skew leeway and must match the IdP lifespan"
created: 2026-09-27
type: lesson
status: seedling
source: "leo-customer360 SSO session fix, 2026-09-27"
tags: [oidc, jwt, token-validation, clock-skew, gotcha]
---

# Token age checks need clock-skew leeway and must match the IdP lifespan

A resource server that enforces its own maximum token age — rejecting when `now - iat > configured_lifetime`, on top of the standard `exp` check — introduces two ways to log users out early.

**1. It must match the IdP's `accessTokenLifespan`.** This local bound is a second, independent expiry. If the API is configured for 30 minutes while the realm issues 60-minute tokens, the API starts refusing tokens the IdP still considers perfectly valid, and users are kicked at 30 minutes with no way to tell from the IdP side that anything is wrong. Whenever both values exist, they are one setting in two places — document them as such, and read them from the same variable if you can.

**2. It needs clock-skew leeway.** `iat` is stamped by the IdP host; the comparison runs on the API host. If the IdP's clock is even slightly ahead, tokens look older than they are — and at the boundary, freshly issued tokens can appear already expired. A small allowance absorbs it:

```python
max_lifespan = _token_lifetime_seconds() + CLOCK_SKEW_LEEWAY_SECONDS  # ~60s
if isinstance(iat, (int, float)) and (now - int(iat)) > max_lifespan:
    return True
```

Worth asking whether the `iat` bound earns its keep at all: `exp` is already authoritative and introspection already confirms the token is active. Keep it as a deliberate backstop against over-long tokens, not as the primary check.

## Related

- [[Keycloak ssoSessionMaxLifespan is an absolute session cap, not an idle timer]]
