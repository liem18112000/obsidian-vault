---
title: "Resource-level consent grants specific instances, not just scopes"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Implement Corporate API Access LUZ-17959 (LUZ)"
tags: [oauth, consent, open-banking, psd2, authorization, confluence-distilled]
---

# Resource-level consent grants specific instances, not just scopes

Standard OAuth scopes answer *"what kind of thing may this client do?"* — `accounts:read`. Open-banking-style consent answers a narrower question: ***which specific resources***, chosen by the user, inside the provider's own trusted interface.

The flow, from a Swiss Corporate API (SIX) integration connecting third-party providers to financial institutions:

1. **Set consent — in the bank's own eBanking.** The user configures **which IBANs are available** to the API. *Only the selected accounts can be accessed.*
2. The user clicks **Authorize Client**, and the browser is redirected to the consent endpoint with the institution identified (`?bank_id=…`).
3. The resulting authorisation grants access to **that set of accounts**, not to the user's whole relationship with the bank.

**Two properties worth separating out:**

- **Consent is set at the resource owner's own trusted surface**, not in the third party's UI. The user grants access while logged into their bank — the one place they can be sure who they are talking to. A consent screen rendered by the party requesting access is weaker by construction.
- **Consent is an allow-list of instances, not of types.** The difference between "this app may read accounts" and "this app may read *these two* accounts" is the entire security value. Scope-based OAuth alone cannot express the second.

**The architectural consequence:** the consent set becomes state the provider must store, expose, version, and enforce on **every** call — not just at token issuance. A token is valid; the accounts it may touch can still change the moment the user edits consent in eBanking. Enforcement therefore has to consult current consent per request, which is closer to [[Authorization has four named parts PAP, PDP, PEP, PIP|Authorization has four named parts: PAP, PDP, PEP, PIP]] than to a scope check.

> [!tip] Give the user a revocation surface in the same place
> If consent is granted in eBanking, it must be **viewable and revocable** there too. A consent model users cannot inspect later is a one-way grant, and it is the part regulators and auditors ask about first.

> [!warning] Aggregator platforms multiply the trust relationships
> Here a single platform (SIX) intermediates between many third-party providers and many financial institutions. That is convenient and it means the consent record, the token, and the enforcement point may sit in three different organisations. Be explicit about which party enforces consent on each call — "the platform handles it" is not an answer you want to discover is wrong.

Source: [[Implement Corporate API Access (LUZ-17959)]] (LUZ, Confluence).

## Related

- [[Authorization has four named parts PAP, PDP, PEP, PIP]]
