---
title: "One chain filename at every altitude lets a reader orient by walking to the root"
created: 2026-09-27
type: model
status: seedling
source: "Confluence: OpenRig's Intent Hierarchy and Refocus (2026-09-27)"
tags: [knowledge-management, ai-agents, openrig, convention, architecture, context-engineering]
---

# One chain filename at every altitude lets a reader orient by walking to the root

openrig's chain-file convention (CE-v2): **one filename, identical at every altitude.** A reader starts where they stand and walks *toward the root*, reading the same-named file at each level.

What that buys, compared with per-level names or a pointer graph:

- **No index to maintain.** There is nothing to update when a level is added — the convention *is* the index.
- **No pointers to follow**, so no broken links and no cycles.
- **Orientation from anywhere.** You do not need to know where you are in the tree to read your context; you just walk up.

The placement rule is the part that is easy to get wrong: **chain files sit on nodes, never on shelves.** `rig`, `pod`, `seat`, `mission`, `slice` are nodes and carry chain files. `rigs/` and `missions/` are *containment* — plural directories that group nodes — and carry nothing. A chain file on a shelf would be context belonging to "all rigs", which is not a position anyone occupies.

Concretely, two chains for two trees: topology carries `LEARNED.md` and `CULTURE.md`; project carries `SPEC.md`, `PROOF.md`, `PROGRESS.md`.

The split between those trees is itself the lesson: **topology defaults ship in source, project context must not** — *"shipping a default would install someone else's project as your context."* Generic norms about how work is done travel; specifics about what is being built do not.

## Related

- [[Knowledge loses meaning when copied out of the position where it was learned]]
- [[Intent composes up the chain, specification stays a leaf property]]

## Related

- [[Knowledge loses meaning when copied out of the position where it was learned]]
- [[Intent composes up the chain, specification stays a leaf property]]
