---
ai_hash: dd2b1e24da691fb7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: LUZ Audit - Basic Understanding Guide + Enhanced Chain-Signature
  Hybrid (2025-11)'
status: seedling
tags:
- audit-logging
- cryptography
- compliance
- rfc3161
- non-repudiation
title: RFC 3161 timestamps outsource the time claim to a party the attacker does not
  control
type: term
---

# RFC 3161 timestamps outsource the time claim to a party the attacker does not control

**RFC 3161** defines a Time-Stamp Protocol: you send a hash of your data to a **Time Stamp Authority (TSA)**, and it returns a token binding that hash to a time, signed with the TSA's certificate. You never send the data itself — only its digest — so the TSA learns nothing about the content.

The point is **whose clock you are trusting**. A timestamp your own service writes is worth exactly as much as your service's integrity; if the attacker owns the box, they own the clock. A TSA token moves the time claim to a third party with its own key and its own audit obligations, which is what turns "we logged it at 10:30" into evidence that survives an adversarial review — the legal term is **non-repudiation**.

Practical shape, from the LUZ Audit design: do **not** stamp every record. TSA calls are network round-trips and the authority usually charges per token. Stamp the **checkpoint** instead — one token per Merkle root covering ~1000 logs — and every log inherits the attestation through its Merkle proof. Cost drops by three orders of magnitude while the guarantee stays per-log.

## Related

- [[Hash chains order by linkage, not by time, so backdated entries still verify]]
- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]

## Related

- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]

%% ai-graph-start %%

**Related notes:**
- [[Hash chains order by linkage, not by time, so backdated entries still verify]]
- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]
- [[LUZ Audit - Basic Understanding Guide]]
- [[Solution - Enhanced Chain-Signature Hybrid]]
- [[Per-record signatures prove authenticity but not completeness, so deletion and reordering go undetected]]

%% ai-graph-end %%