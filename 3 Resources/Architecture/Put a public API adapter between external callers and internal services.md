---
ai_hash: 3de5159072b7e41f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: One API Module Responsibilities (LUZ)'
status: seedling
tags:
- api-design
- adapter
- facade
- authentication
- microservices
- confluence-distilled
title: Put a public API adapter between external callers and internal services
type: concept
---

# Put a public API adapter between external callers and internal services

Exposing internal services directly to external consumers couples your public contract to your internal decomposition. A dedicated **adapter service** in front of them breaks that coupling, and it should own two jobs explicitly.

From a module-responsibility register:

> **`luz-public-api-adapter`** — *translates the external (easier) requests for the internal (more complex) APIs* · *handles authentication from the outside world to internal KLARA*

**Job 1 — translation.** External callers get an API shaped around *what they want to do*; internally that may fan out across several services with a more complex contract. Without the adapter, every external integrator has to learn your service boundaries, and every internal refactor becomes a breaking public change.

**Job 2 — the authentication boundary.** External credentials are converted to internal ones at exactly one place. Internal services then only ever deal with internal identity, and the code that handles untrusted callers lives in one reviewable component rather than being duplicated across every service that might be reachable.

**Why one component doing both is right.** They are the same boundary. Translating an external request means establishing *who is asking* before you can fan out on their behalf — so splitting auth from translation just means two things that must agree about the same trust transition.

> [!tip] The register is as useful as the pattern
> The source is a plain table: **module · responsibility · comment · team · implemented**. Two columns earn their place beyond the obvious — **team** (who owns it, so a question has an address) and **implemented** (what is real versus planned, which a diagram never shows). A one-line responsibility per service is the cheapest architecture document that stays true, and writing it surfaces the services nobody can describe in one line.

> [!warning] Adapters accrete business logic
> "Translate and authenticate" drifts into "and also validate, and enrich, and cache, and special-case this partner". Once business rules live in the adapter you have a second, undocumented domain model at the edge. Keep the rule explicit: the adapter reshapes and authenticates; it does not decide. Anything that makes a business decision belongs behind it.

Related: [[Exchange a partner IdP token by introspecting it, never by trusting it]] — what job 2 looks like in detail.

Source: [[One API Module Responsibilities]] (LUZ, Confluence).

## Related

- [[Exchange a partner IdP token by introspecting it, never by trusting it]]

%% ai-graph-start %%

**Related notes:**
- [[One API Module Responsibilities]]
- [[Roles and Permissions check for accessing public API]]
- [[Share features as vertical slices with app-owned routes and an injected adapter]]
- [[Catalog for 3rd party system API]]

%% ai-graph-end %%