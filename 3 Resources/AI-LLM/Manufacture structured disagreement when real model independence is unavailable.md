---
ai_hash: cbeff219fa61cb9e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- LLM
- Agreeableness
- Reviewer
- Andrej Karpathy
- LLM Council
- OpenAI
- Anthropic
- Google
- xAI
- Training data differences
- Alignment differences
- Single-vendor adaptation
- Thinking-lens sub-agents
- Contrasting prompts
- Prompt-defined lenses
- Contrarian (Persona)
- Lens contrast (Mechanic)
- Anonymised peer review (Mechanic)
- Chair empowered to dissent (Mechanic)
- Prompt-induced diversity
- Lab diversity
- Model blind spots
- Structured disagreement
- Real model independence
- Proposal An LLM Council for ePost – multi-lens deliberation in Claude Code
source: 'Confluence: Proposal An LLM Council for ePost (TK)'
status: seedling
tags:
- llm
- multi-agent
- llm-council
- sycophancy
- claude-code
- evaluation
- confluence-distilled
title: Manufacture structured disagreement when real model independence is unavailable
type: concept
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

%% ai-graph-start %%

**Related notes:**
- [[Proposal An LLM Council for ePost – multi-lens deliberation in Claude Code]]
- [[LLM-as-a-judge biases position, verbosity, self-enhancement]]
- [[AI self-critique loop - a post-generation critic pass rates the artifact and feeds the next run]]
- [[Find then adversarial-refute verify pass cuts AI reviewer false positives]]
- [[A coding-agent prompt needs codebase anchors and stated house style]]

**Relations:**
- LLM — *exhibits* — Agreeableness
- Agreeableness — *renders* — LLM useless as Reviewer
- Andrej Karpathy — *proposed* — LLM Council
- LLM Council — *mitigates* — Agreeableness
- LLM Council — *replaces* — single model with several
- LLM Council — *uses models from* — OpenAI
- LLM Council — *uses models from* — Anthropic
- LLM Council — *uses models from* — Google
- LLM Council — *uses models from* — xAI
- LLM Council — *employs* — Anonymised peer review (Mechanic)
- LLM Council — *includes* — chair model
- chair model — *synthesizes* — verdict
- LLM Council — *derives diversity from* — Training data differences
- LLM Council — *derives diversity from* — Alignment differences
- Single-vendor adaptation — *is an adaptation of* — LLM Council
- Single-vendor adaptation — *uses* — one model
- Single-vendor adaptation — *employs* — Thinking-lens sub-agents
- Thinking-lens sub-agents — *are driven by* — Contrasting prompts
- Single-vendor adaptation — *derives diversity from* — Prompt-defined lenses
- Contrarian (Persona) — *is an example of* — Thinking-lens sub-agents
- Lens contrast (Mechanic) — *is a core mechanic* — LLM Council
- Anonymised peer review (Mechanic) — *is a core mechanic* — LLM Council
- Chair empowered to dissent (Mechanic) — *is a core mechanic* — LLM Council
- Lens contrast (Mechanic) — *ensures* — voices are constructed to disagree
- Anonymised peer review (Mechanic) — *suppresses* — deference
- Chair empowered to dissent (Mechanic) — *prevents* — consensus-by-default
- Prompt-induced diversity — *is shallower than* — Lab diversity
- Prompt-induced diversity — *shares* — Model blind spots
- Structured disagreement — *is manufactured when* — Real model independence is unavailable
- Single-vendor adaptation — *is a form of* — Structured disagreement
- Proposal An LLM Council for ePost – multi-lens deliberation in Claude Code — *is a source for* — this note
- Proposal An LLM Council for ePost – multi-lens deliberation in Claude Code — *is related to* — this note

%% ai-graph-end %%