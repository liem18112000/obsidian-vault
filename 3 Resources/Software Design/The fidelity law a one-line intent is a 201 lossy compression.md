---
title: "The fidelity law: a one-line intent is a 20:1 lossy compression"
created: 2026-09-25
type: concept
status: seedling
source: "openrig docs/reference/product-management-pass.md, 2026-09-25"
tags: [openrig, delegation, specs, context-engineering]
---

# The fidelity law: a one-line intent is a 20:1 lossy compression

A one-line statement of intent is roughly a 20:1 lossy compression of the design behind it. Build from it alone and you match the original only by coincidence.

openrig's rule for handing work to an agent or a teammate. The build is a **lossy compression pipeline** — product intent → spec → code — and each stage is a narrower reader than the one above. The consequence is the sharp part:

> Nothing leaks downward by proximity. A dimension the owner holds and does not WRITE AND ROUTE is deleted from the product — and **the deletion is silent**: the downstream agent invents a plausible replacement rather than raising a hand.

So the fix is not a better summary, it is depositing the full design contract INLINE at the moment the work is minted: the owner's words verbatim, the settled do-not-reopen decisions, the evidence, the shape of done, the anti-goals, and the complete reading list with one line of why for each.

**The measuring stick:** the spec alone, plus only what it explicitly routes to, reproduces the design intent in a reader who has none of your context.

**Corollary — "a file's existence is not a route."** The downstream reader's world is the spec; their curiosity does not extend past it. Nothing load-bearing may depend on a reader's initiative. If it matters, route it explicitly.

Practical use: it is an argument AGAINST adding a thin one-line `intent` field to a data model. Downstream code will read the cheap field instead of the rich one, and the compression loss becomes structural.

Related: [[Price one layer lower before accepting a fix]] · [[CONTEXT-GAP vs JUDGMENT-GAP: dispose every miss by cause]]

## Related

- [[Price one layer lower before accepting a fix]]
- [[CONTEXT-GAP vs JUDGMENT-GAP: dispose every miss by cause]]
