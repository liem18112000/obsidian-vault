---
ai_hash: 6d80fd35555c4f22
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-01
entities: []
source: github.com/block/buzz ARCHITECTURE.md 2026-08-01
status: seedling
tags:
- nostr
- protocol-design
- forward-compatibility
- buzz
title: Nostr custom kinds as a feature-extension mechanism
type: concept
---

# Nostr custom kinds as a feature-extension mechanism

In Nostr, every event carries an integer `kind` that names its type, and clients are required to **ignore events whose kind they do not recognize**. This turns "add a feature" into "define a new kind number": new event types can ship without a coordinated protocol version bump, and **old clients see nothing and break nothing**.

Buzz (block/buzz) leans on this directly — its ARCHITECTURE.md states adding a feature = defining a new kind, and it reserves the 40000-49999 range for custom Buzz kinds (e.g. stream messages, forum posts, workflow-execution events) alongside standard Nostr ranges (0-9999 standard, 10000-19999 replaceable, 20000-29999 ephemeral, 30000-39999 parameterized-replaceable).

Why it matters: it is a **forward-compatible extension mechanism** — the schema is open by construction, so independent clients and servers evolve without lockstep upgrades. The trade-off is there is no central registry enforcing kind meanings, so kind-number collisions/semantics are a social/spec convention, not a guarantee.

See [[Buzz (block/buzz) is a Nostr-based workspace where humans and AI agents are peers]].

## Related

- [[Buzz (block/buzz) is a Nostr-based workspace where humans and AI agents are peers]]

%% ai-graph-start %%

**Related notes:**
- [[Buzz (blockbuzz) is a Nostr-based workspace where humans and AI agents are peers]]
- [[blockbuzz architecture a Nostr-relay hive mind for humans and AI agents]]

%% ai-graph-end %%