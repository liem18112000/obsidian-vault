---
ai_hash: 95f444c6149db278
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Security Risk - Luz-jwt Permission By Pass (2026-02-11)'
status: seedling
tags:
- security
- http
- status-codes
- authorization
- api-design
- gotcha
title: Returning 401 for a permission failure causes infinite login loops
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[A null-guarded tenant check fails open, so a renamed path parameter disables isolation]]
- [[Security Risk - Luz-jwt Permission By Pass]]
- [[Security Risk - Luz-jwt Permission By Pass]]
- [[Blanket 401 auto-logout swallows the login endpoint's own error]]
- [[Luz 403 Not allowed means the token has no tenant claim, not a missing permission]]

%% ai-graph-end %%