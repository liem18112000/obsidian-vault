---
title: "Returning 401 for a permission failure causes infinite login loops"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Security Risk - Luz-jwt Permission By Pass (2026-02-11)"
tags: [security, http, status-codes, authorization, api-design, gotcha]
---

# Returning 401 for a permission failure causes infinite login loops

The two codes answer different questions, and swapping them creates a client-side failure loop:

- **401 Unauthorized** — *"I do not know who you are."* The credentials are missing, expired, or invalid. The correct client response is **re-authenticate**.
- **403 Forbidden** — *"I know exactly who you are, and you may not do this."* Authentication succeeded; authorisation failed. The correct client response is **stop and show an error**.

The `luz_jwt` filter returned **401 on a tenant mismatch** — a permission decision dressed as an identity failure. Three things follow, all observed:

- **Infinite login loops.** The client reads 401 as "token expired", refreshes, retries with a perfectly valid token, gets 401 again — forever. The token was never the problem.
- **Token-refresh storms.** Any interceptor that auto-refreshes on 401 turns one forbidden request into sustained load on the auth service. A single bad client link can look like an attack.
- **Misleading UX.** The user is told their session expired and logs in again, which cannot possibly help.

Rule of thumb: **if presenting a fresh, valid credential would not change the outcome, it is not a 401.**

Worth stating because the mistake is so common it reads as idiomatic — many codebases return 401 for everything auth-adjacent, and the cost only appears once a client has retry logic.

## Related

- [[A null-guarded tenant check fails open, so a renamed path parameter disables isolation]]

## Related

- [[A null-guarded tenant check fails open, so a renamed path parameter disables isolation]]
