---
title: "Thinking tokens are billed as output, so effort level is a cost lever"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Manual Prompt Compression Techniques - Deep Dive (2026-04-13)"
tags: [prompt-engineering, chain-of-thought, llm, cost-optimization, claude, reasoning]
---

# Thinking tokens are billed as output, so effort level is a cost lever

Chain-of-thought reasoning tokens are billed at **output** rates — the expensive ones — and unconstrained they can run **10× the length of the actual answer**. The failure modes are predictable: the model repeats itself, hedges, wanders, and explores dead ends that never reach the response.

That makes reasoning depth a **budget decision per request**, not a global setting. Claude exposes it directly:

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=4096,
    thinking={"type": "adaptive"},
    output_config={"effort": "low"},     # "low" | "medium" | "high"
    messages=[...],
)
```

- **low** — simple, mechanical tasks. Fastest and cheapest.
- **medium** — the sensible default.
- **high** — genuinely hard problems only.

The mistake worth naming: treating high effort as "better quality" and setting it globally. On a task that did not need it you pay several times over for reasoning that reaches the same answer — and long reasoning on a simple task can actively hurt, as the model talks itself out of a correct first instinct.

Two cheap companions on the same lever: **`max_tokens`** caps the worst case, and **stop sequences** end generation as soon as the answer is structurally complete instead of letting the model round off with a summary nobody reads.

## Related

- [[Few-shot cost grows linearly while accuracy flattens, so escalate examples in steps]]

## Related

- [[Few-shot cost grows linearly while accuracy flattens, so escalate examples in steps]]
