---
title: "Grade identity assurance by what was proven, not a verified boolean"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: E-Post API technical documentation (LUZ)"
tags: [identity, verification, kyc, levels-of-trust, onboarding, confluence-distilled]
---

# Grade identity assurance by what was proven, not a verified boolean

"Is this user verified?" is the wrong question — verification is not boolean. Grade it by **what was actually proven and how hard that is to fake**, then let each operation require a level.

A three-tier scheme from a digital-mail platform:

| Level | What was proven | Method |
|---|---|---|
| **BRONZE** | Controls an email address | Entered a code or clicked a link sent by email |
| **SILVER** | Controls a **physical address** | Entered a code delivered by **physical letter** |
| **GOLD** | Is a specific **person** | Online identification: photo of an official identity document plus a live selfie |

The ordering is not arbitrary — each step raises the cost and the physical commitment of faking it. Email is free and instant. A letter takes days and requires access to a real postbox. An identity document plus a liveness check ties the account to a legally identifiable human.

**Why a ladder beats a flag:**

- **Operations can require a level.** Reading a marketing message needs BRONZE; receiving legally-binding mail plausibly needs SILVER or GOLD. One boolean forces every action to accept the weakest proof you ever accept for anything.
- **Users can enter cheaply and upgrade later.** Requiring GOLD at signup kills conversion; requiring it at the moment it matters does not.
- **It maps to the delivery channel.** SILVER exists because the product delivers to a **postal address** — the level proves exactly the attribute the operation depends on. That is the design rule: *verify the attribute you are about to rely on*, not identity in the abstract.

**Credentials are a separate axis.** The same spec distinguishes a **primary credential** — email, mobile number, postal address, participant ID, key address hash — which must be supplied together with **other non-unique credentials** (first name, last name, street, number, ZIP, city) to match a person. Identity *matching* and identity *assurance* are different problems: matching finds the right record, the level of trust says how much you believe it is really them.

> [!tip] Name the levels after the proof, not the tier
> BRONZE/SILVER/GOLD reads nicely but tells a new engineer nothing. Keep a table that maps each tier to the concrete artefact — *"SILVER = postal code entered from a physical letter"* — next to the enum. Otherwise the levels drift into vibes and someone adds PLATINUM.

> [!warning] A level is a claim about the past
> GOLD means the check passed *once*. It does not mean the account is not currently compromised, nor that the document was not forged. Pair levels with session-level controls; assurance at enrolment and assurance at this request are different guarantees.

Related: [[RBAC is coarse-grained by role, ABAC is fine-grained by attribute]] — level of trust is exactly the kind of user attribute an ABAC policy consumes.

Source: [[E-Post API - technical documentation]] (LUZ, Confluence).

## Related

- [[RBAC is coarse-grained by role, ABAC is fine-grained by attribute]]
