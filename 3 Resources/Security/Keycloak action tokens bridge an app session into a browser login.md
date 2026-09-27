---
ai_hash: ca8f2a5ea6657c0e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Proof of Concept Auto login with Keycloak (TP2020)'
status: seedling
tags:
- keycloak
- sso
- action-token
- authentication
- magic-link
- confluence-distilled
title: Keycloak action tokens bridge an app session into a browser login
type: howto
---

# Keycloak action tokens bridge an app session into a browser login

A native app and a web page share an identity provider but not a session: open a web URL from inside the app and the user, already authenticated in the app, gets a login form in the mobile browser. Keycloak can bridge this with an **action token** plus a custom authentication flow that has no password step.

**The flow, `auto login`, is two executions:**

| Execution | Requirement | Effect |
|---|---|---|
| **Cookie** | Alternative | If a Keycloak SSO cookie already exists, use it and skip the rest |
| **Username Validation** | Required | Otherwise authenticate from the username carried in the token |

There is deliberately **no password validation** — the app never has the password.

**The token** is a custom `DefaultActionToken` subclass; its `TOKEN_TYPE` becomes the `typ` claim, which is how Keycloak routes it to your handler:

```java
public class AutoLoginActionToken extends DefaultActionToken {
    public static final String TOKEN_TYPE = "auto-login";

    public AutoLoginActionToken(String userId, int absoluteExpirationInSecs,
                                String authenticationSessionId) {
        super(userId, TOKEN_TYPE, absoluteExpirationInSecs, null, authenticationSessionId);
    }
    private AutoLoginActionToken() {}   // required for deserialization
}
```

A matching **handler** receives the token in `handleToken`, pulls the username from the context, and drives the `auto login` flow; a **factory** registers the handler for that token type. The app mints the token, opens the browser at the action-token URL, and the browser lands authenticated. It only works while the app's user session is still valid.

> [!warning] A flow with no password step is an authentication bypass if the token is weak
> Everything rests on the action token: possession of it *is* the credential. The source note flags the gap itself — *"we can enhance this by also validating the access token of the user who wants to create the auto login flow."* That enhancement is not optional in production. Minimum bar before shipping:
> - **Short absolute expiry** (seconds, not minutes) — it is a hand-off, not a session.
> - **Single use** — bind to `authenticationSessionId` and invalidate on redemption, so a token captured from a log or `Referer` header cannot be replayed.
> - **Verify the requester** — the endpoint minting the token must prove the caller holds a valid access token *for that same user*, or any authenticated caller can mint a login for anyone.
> - **Never log the URL.** An action-token URL in an access log, crash report, or analytics payload is a live credential.

> [!tip] The generalisable pattern
> This is the same shape as a magic-link login: a short-lived, single-use, user-bound token that substitutes for the credential at one specific handoff. The security properties come entirely from those three adjectives — drop any one and it becomes a bypass.

Related: [[Exchange a partner IdP token by introspecting it, never by trusting it]] — the same rule: never let possession alone be sufficient without verification.

Source: [[Proof of Concept Auto login with Keycloak]] (TP2020, Confluence).

## Related

- [[Exchange a partner IdP token by introspecting it, never by trusting it]]

%% ai-graph-start %%

**Related notes:**
- [[Proof of Concept Auto login with Keycloak]]
- [[Copy Proof of Concept Passwordless account login with Keycloak]]
- [[Proof of Concept Passwordless account login with Keycloak]]
- [[Auto login in myLife and ePost private web clients]]
- [[OIDC federation with just-in-time provisioning hinges on the attribute join key]]

%% ai-graph-end %%