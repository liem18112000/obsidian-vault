---
title: "Hash chains order by linkage, not by time, so backdated entries still verify"
created: 2026-09-27
type: argument
status: seedling
source: "Confluence: LUZ Critical Concerns - Brief Summary (2025-11-17)"
tags: [audit-logging, security, cryptography, timestamps, luz-audit]
---

# Hash chains order by linkage, not by time, so backdated entries still verify

A fingerprint chain enforces **linkage order** — entry *n* references entry *n−1* — but it never checks that timestamps increase along that linkage. The verifier confirms the hashes line up; it does not confirm that the clock did.

So an attacker who can append (or an application bug that processes out of order) can insert a record carrying **any `timestamp` they like**, including one backdated before events already in the chain. The chain still verifies. The log now asserts, with cryptographic confidence, a chronology that never happened — which is worse than no timestamp at all, because downstream readers trust it.

Two cheap defences, used together:

- a **monotonic sequence number per tenant**, signed as part of the record, so position is an explicit signed claim rather than an inference from linkage;
- a **trusted timestamp** from outside the system — an RFC 3161 token from a Time Stamp Authority — so the time claim is attested by a party the attacker does not control. See [[RFC 3161 timestamps outsource the time claim to a party the attacker does not control]].

Generalisation: **a chain proves relative order of writes, never absolute time.** If you need "this existed by date X", something external has to say so.

## Related

- [[A hash chain proves integrity but not authorship, so a database admin can silently rebuild it]]
- [[RFC 3161 timestamps outsource the time claim to a party the attacker does not control]]

## Related

- [[RFC 3161 timestamps outsource the time claim to a party the attacker does not control]]
