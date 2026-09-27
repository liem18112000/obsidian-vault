---
title: "Manufacture structured disagreement when real model independence is unavailable"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Proposal An LLM Council for ePost (TK)"
tags: [llm, multi-agent, llm-council, sycophancy, claude-code, evaluation, confluence-distilled]
---

# Manufacture structured disagreement when real model independence is unavailable

A single LLM is **agreeable**, and agreeableness makes it useless as a reviewer. Ask "should we ship this?" and you get five reasons to ship. Ask "is this a bad idea?" about the *same* proposal and you get five reasons it is not. The framing of the question, not the merit of the idea, determines the verdict — which is fine for drafting an email and dangerous for architecture and product decisions.

Andrej Karpathy's **LLM Council** attacks this by replacing one model with several from different labs, having them peer-review each other **anonymously**, and letting a chair model synthesise a verdict. The diversity comes from differences in training data and alignment.

**The adaptation worth knowing:** you can keep the protocol and swap the *source* of diversity. Instead of four labs, use five **thinking-lens sub-agents driven by contrasting prompts** inside one vendor's stack:

| Karpathy's setup | Single-vendor adaptation |
|---|---|
| Four labs (OpenAI, Anthropic, Google, xAI), one prompt each | One model, five lens prompts |
| Diversity from training-data and alignment differences | Diversity from prompt-defined lenses that contrast **by design** |

Each lens is a persona with a job, not a topic. The Contrarian, for instance:

> You are the Contrarian. Actively look for what is wrong, what is missing, what will fail. Assume the idea has a fatal flaw and dig until you find it. If everything looks solid, dig deeper. You are not a pessimist — you are the friend who saves the user from a bad deal by asking the question they are avoiding.

**The three mechanics that carry the value** — and they survive the swap:

1. **Lens contrast** — the voices are constructed to disagree, so agreement between them is evidence rather than an artifact of one prompt's framing.
2. **Anonymised peer review** — reviewers judge the argument without knowing whose it is, which suppresses deference.
3. **A chair empowered to dissent** — synthesis is not averaging. A chair that can overrule the majority prevents the council collapsing into consensus-by-default.

> [!warning] Be honest about the ceiling
> Prompt-induced diversity is **shallower** than lab diversity — five prompts over one model share its blind spots, and no lens can see what the base model cannot. The proposal accepts this explicitly rather than claiming equivalence. That is the right way to state it: the protocol's other mechanics carry most of the value, so you keep most of the benefit at a fraction of the integration cost.

**The generalisable idea:** when you cannot get real independence, *manufacture structured disagreement* and be explicit about how much weaker it is. A council of one model wearing five hats still beats one agreeable voice — just do not mistake it for genuine independent verification.

Source: [[Proposal An LLM Council for ePost – multi-lens deliberation in Claude Code]] (TK, Confluence).

## Related

- [[Proposal An LLM Council for ePost – multi-lens deliberation in Claude Code]]
