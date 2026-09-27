---
title: "A hash chain proves integrity but not authorship, so a database admin can silently rebuild it"
created: 2026-09-27
type: argument
status: seedling
source: "Confluence: LUZ Critical Concerns - Brief Summary (2025-11-17)"
tags: [audit-logging, security, cryptography, threat-model, luz-audit]
---

# A hash chain proves integrity but not authorship, so a database admin can silently rebuild it

A SHA-256 fingerprint chain answers "was this sequence altered?" but it cannot answer "**who wrote this?**". The hash function is public and keyless, so anyone who can write to the audit collection can also recompute a perfectly valid chain.

That makes the **database administrator the unhandled threat**. The attack is not subtle: delete the incriminating entries, recompute fingerprints forward from the deletion point, and the chain verifies cleanly. The audit log's own verification routine reports "intact" precisely because the attacker had everything needed to make it so. Evidence destruction becomes undetectable, which is the one property an audit log exists to prevent.

The fix is an **asymmetric signature**: each record (or checkpoint) is signed with a private key that lives outside the database — in a KMS or HSM. A DBA can still delete rows, but cannot produce a valid signature for the rewritten sequence, so the forgery is detectable even with full database access. Keyed integrity (HMAC) is weaker here: whoever verifies also holds the key and could have forged it.

Rule of thumb: **if the verifier's secret is reachable from the attacker's position, the mechanism proves nothing.**

## Related

- [[A hash-chained audit log cannot be written in parallel]]
- [[Hash chains order by linkage, not by time, so backdated entries still verify]]

## Related

- [[A hash-chained audit log cannot be written in parallel]]
- [[Hash chains order by linkage]]
- [[not by time]]
- [[so backdated entries still verify]]
