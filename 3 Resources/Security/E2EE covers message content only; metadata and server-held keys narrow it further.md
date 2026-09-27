---
ai_hash: d9594e75fb010219
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Communities Privacy Concept (ISMS)'
status: seedling
tags:
- e2ee
- encryption
- privacy
- matrix
- metadata
- dpia
- confluence-distilled
title: E2EE covers message content only; metadata and server-held keys narrow it further
type: concept
---

# E2EE covers message content only; metadata and server-held keys narrow it further

"The app is end-to-end encrypted" is almost never the whole truth. E2EE covers **message content**, and the boundary around it is narrower than teams assume — narrow enough that a privacy concept or DPIA that repeats the marketing line will be wrong.

From a Matrix-based messaging privacy assessment, the accurate scope:

- **Encrypted:** messages and attachments carried in *message events*.
- **Not encrypted, even inside an encrypted room:** reactions, read receipts, and other communication events.
- **Not encrypted at all:** technical metadata — sender and recipient IDs, room IDs, timestamps, device information.

The page is more instructive for what it **corrects**. An earlier version claimed E2EE applied to *"messages, attachments, reactions, read receipts, and other communication events"*; that line is struck through and replaced with the narrower, accurate one. Overstating the boundary is the default error, and it survives until someone checks the protocol rather than the summary.

> [!warning] Metadata is often the more sensitive asset
> Who talked to whom, when, how often, and from which device reconstructs a relationship graph without reading a single message. For B2C and C2C messaging that graph can be more revealing than content. A privacy concept that says "content is E2EE" and stops has skipped the part with the higher protection need.

**The second dilution: server-side key storage.** In this design, keys are *generated on the device* but **stored server-side**, and retrieved at login so a user can decrypt their rooms on a new device. That is a real usability requirement — without it, losing a device means losing history. But it changes the threat model: the operator holds material that participates in decryption, so "we cannot read your messages" now rests on operational controls around that store rather than on cryptography alone. Say which one you are relying on.

**A third boundary worth stating explicitly:** unencrypted rooms. The same assessment notes that in unencrypted *public* rooms, content **can** be accessible depending on configuration. If your product offers both encrypted and unencrypted rooms, "the app is E2EE" is false for part of it.

> [!tip] How to write this section honestly
> Enumerate the event types and state encrypted/not for each, rather than making one claim about "the app". Then name where the keys live and who can reach them. Those two lists are what an auditor, a DPIA, and a protection-needs assessment all actually need — and writing them is how you discover the overclaim before a reviewer does.

Related: [[Envelope encryption with Vault transit keeps Vault off the data path]] — a different key-custody trade, made explicitly.

Source: [[Communities Privacy Concept – Working Page for PO and Engineering]] (ISMS, Confluence).

## Related

- [[Envelope encryption with Vault transit keeps Vault off the data path]]

%% ai-graph-start %%

**Related notes:**
- [[Communities Privacy Concept – Working Page for PO and Engineering]]
- [[Envelope encryption with Vault transit keeps Vault off the data path]]

%% ai-graph-end %%