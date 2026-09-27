---
title: "Intent composes up the chain, specification stays a leaf property"
created: 2026-09-27
type: model
status: seedling
source: "Confluence: OpenRig's Intent Hierarchy and Refocus (2026-09-27)"
tags: [knowledge-management, ai-agents, openrig, context-engineering, documentation]
---

# Intent composes up the chain, specification stays a leaf property

openrig's composition rule for `SPEC.md`:

> **Intent in frontmatter composes up the chain; specification is a leaf property.**

Two different things, deliberately stored differently. **Intent accumulates** — walking from a slice toward the project, each level's frontmatter adds a layer of *why*, and the full chain is the context. **Specification does not** — only the leaf carries the concrete contract of what to build.

The failure this prevents is restating intent at every level. That looks like good documentation and is actually drift: five paraphrases of the same goal that diverge on the first edit, with no way to tell which is authoritative.

The related fidelity law is sharper, and it is the one worth carrying:

> A one-line intent is a **~20:1 lossy compression**.

So "an intent sentence per altitude" is explicitly the **anti-pattern**, not the mechanism. A one-liner is a *handle* for something already written down, not a substitute for it — which is why the full design contract is deposited **inline in the leaf**. The summary is the index; the leaf is the source of truth.

The precedence stack that makes this concrete, from openrig's operating-posture resolver:

```
qitem → workstream → mission → project → rig → global host
```

Nearest altitude wins; everything above supplies defaults.

Practical read: when writing layered context (CLAUDE.md, skill files, agent specs), **let the upper levels say why and the leaf say what** — and resist the urge to summarise the leaf upward.

## Related

- [[One chain filename at every altitude lets a reader orient by walking to the root]]
- [[Knowledge loses meaning when copied out of the position where it was learned]]

## Related

- [[One chain filename at every altitude lets a reader orient by walking to the root]]
