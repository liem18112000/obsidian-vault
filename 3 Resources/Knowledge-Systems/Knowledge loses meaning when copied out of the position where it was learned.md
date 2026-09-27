---
title: "Knowledge loses meaning when copied out of the position where it was learned"
created: 2026-09-27
type: argument
status: seedling
source: "Confluence: OpenRig's Intent Hierarchy and Refocus (2026-09-27)"
tags: [knowledge-management, ai-agents, openrig, test-agent-v2, architecture, zettelkasten]
---

# Knowledge loses meaning when copied out of the position where it was learned

openrig's `lore-routing.md` states a rule that cuts against the instinct to deduplicate knowledge:

> The value is the combination of the content **and where it was learned**; copying the bytes into a shared library destroys that distinction.

A lesson learned at a particular position — this repo, this role, this mission — carries its scope implicitly. Lift it into a central `lessons/` directory and the bytes survive while the scope is lost. It now reads as universally applicable, gets retrieved in contexts where it is wrong, and nobody can tell which of the ten similar entries applies to them.

This inverts the usual refactoring reflex. **Duplication of knowledge is not the same failure as duplication of code.** Two copies of a function must agree forever; two lessons learned at two positions are *different facts about different positions* that happen to share wording.

The structural consequence is that knowledge should be **stored at the node where it was learned** and **composed by walking toward the root** at read time — not gathered into a shared store at write time. Retrieval assembles the scope; storage preserves it.

In Test-Agent-V2 this was the expensive lesson: the team had centralised captured lessons and then spent **six separate mitigations** trying to restore the scope they had flattened away — and a live defect survived anyway, where the semantic arm of `recall_lessons` could never return an auto-captured lesson.

Applies directly to a PARA/Zettelkasten vault: a note's folder *is* part of its meaning, which is why a note that could live anywhere is usually one that has not been made specific enough.

## Related

- [[One chain filename at every altitude lets a reader orient by walking to the root]]
- [[Intent composes up the chain, specification stays a leaf property]]

## Related

- [[One chain filename at every altitude lets a reader orient by walking to the root]]
