---
ai_hash: 83c99d3b926b3762
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Solution - Enhanced Chain-Signature Hybrid (2025-11-04)'
status: seedling
tags:
- audit-logging
- security
- cryptography
- design-tradeoff
- luz-audit
title: Per-record signatures prove authenticity but not completeness, so deletion
  and reordering go undetected
type: argument
---

# Per-record signatures prove authenticity but not completeness, so deletion and reordering go undetected

Independently signing each audit record fixes authorship and parallelises writes, but it **silently drops two properties the chain was providing**, and the loss is easy to miss because every individual signature still verifies.

What a set of independently-signed records cannot detect:

- **Deletion.** Remove a record and nothing is inconsistent — the survivors are all validly signed. There is no "hole" to find, because nothing referenced the missing one.
- **Reordering.** Records carry no relative position, so replaying them in a different order produces an equally valid log.

A signature proves *"this record is authentic"*. It says nothing about *"this is the complete and correctly-ordered set of records"*. **Completeness is a property of the linkage, not of the records.**

The LUZ hybrid restores both cheaply without going back to a serial chain: each record carries a **monotonic `sequenceNumber` per tenant** and the **`previousSignature`**, and both are inside the signed payload. A gap in the sequence is now evidence of deletion; a mismatched `previousSignature` is evidence of reordering — and neither requires recomputing anything from genesis.

Design rule: **when replacing a chain with signatures, ask what the chain was proving besides integrity.** Usually it is completeness, and that has to be re-added deliberately.

## Related

- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]
- [[A hash chain proves integrity but not authorship, so a database admin can silently rebuild it]]

## Related

- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]

%% ai-graph-start %%

**Related notes:**
- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]
- [[A hash-chained audit log cannot be written in parallel]]
- [[Solution - Enhanced Chain-Signature Hybrid]]
- [[A hash chain proves integrity but not authorship, so a database admin can silently rebuild it]]
- [[Hash chains order by linkage, not by time, so backdated entries still verify]]

%% ai-graph-end %%