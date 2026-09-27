---
title: "Harness Engineering: Designing Reliable AI Systems"
created: 2026-04-06
updated: 2026-04-06
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49299914833/Harness+Engineering+Designing+Reliable+AI+Systems
confluence_id: "49299914833"
confluence_path: "Team Kepler > Developer note > AI Research > Agentic and LLM"
tags: [confluence, ai-agents, search]
---

# Harness Engineering: Designing Reliable AI Systems

*Confluence source · Team Kepler › Developer note › AI Research › Agentic and LLM · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49299914833/Harness+Engineering+Designing+Reliable+AI+Systems) · updated 2026-04-06*

## Executive Summary

**Harness Engineering** is the discipline of designing everything around an AI model — tools, guardrails, feedback loops, memory, state management, security, and orchestration — to make AI agents reliable in production. The term was coined by **Mitchell Hashimoto** (co-founder of HashiCorp/Terraform) in February 2026.

The core equation is simple:

```
Agent = Model + Harness
```

The model is what **thinks**. The harness defines **what it thinks about**.

## What is Harness Engineering?

### Simple Definition

Think of it like this:

|                    |                                             |
|--------------------|---------------------------------------------|
| Analogy            | AI Equivalent                               |
| A horse            | The AI model (powerful but unpredictable)   |
| Reins, saddle, bit | The harness (controls direction & behavior) |
| The rider          | The human operator                          |
| The destination    | The task/goal                               |

Or in computer terms:

|                  |                    |
|------------------|--------------------|
| Computer Analogy | AI Equivalent      |
| CPU              | The Model          |
| RAM              | The Context Window |
| Operating System | The Harness        |
| Application      | The Agent          |

### What the Harness Includes

|  |  |  |
|----|----|----|
| Component | Purpose | Example |
| **Tools** | What the agent can do | File editors, web browsers, APIs |
| **Guardrails** | What the agent cannot do | Permission gates, validation layers |
| **Feedback Loops** | Self-correction mechanisms | Linting, testing, evaluator agents |
| **Memory** | Persistent knowledge | MEMORY.md, session transcripts |
| **State Management** | Track progress across sessions | Task boards, plan files |
| **Context Management** | Right info at right time | RAG, history compression |
| **Security** | Access control & safety | 23 validation layers for bash commands |
| **Orchestration** | Multi-agent coordination | Planner-Generator-Evaluator pattern |

### The Core Principle

> "Whenever an agent makes a mistake, engineer a solution ensuring it never repeats that mistake."
> — **Mitchell Hashimoto**, [My AI Adoption Journey](https://mitchellh.com/writing/my-ai-adoption-journey)

![[image-20260406-064504.png]]

## The Three-Stage Evolution

The AI engineering discipline has evolved through three distinct phases:

### The Analogy

|  |  |  |
|----|----|----|
| Stage | Analogy | What You Optimize |
| **Prompt Engineering** | Writing a better email | The single instruction |
| **Context Engineering** | Attaching the correct files to the email | The information provided |
| **Harness Engineering** | Designing the entire office — processes, people, tools, quality control | The complete work environment |

### Nested Relationship

These three are not replacements — they are **nested layers**:

![[image-20260406-064604.png]]

## Comparison Table

### Prompt Engineering vs Context Engineering vs Harness Engineering

|  |  |  |  |
|----|----|----|----|
| Dimension | Prompt Engineering | Context Engineering | Harness Engineering |
| **Era** | 2022–2024 | 2025 | 2026+ |
| **Core Question** | "How do I phrase this?" | "What info does the model need?" | "How does the whole system work?" |
| **Scope** | Single interaction | Information pipeline | Complete ecosystem |
| **Focus** | Input text optimization | Dynamic context assembly | Infrastructure + orchestration |
| **Analogy** | Writing a good email | Attaching the right files | Designing the entire office |
| **Key Techniques** | System prompts, few-shot, chain-of-thought | RAG, memory, tool definitions | Guardrails, feedback loops, multi-agent, state |
| **Who Benefits** | Any LLM user | App/product builders | Production AI systems |
| **Failure Mode** | Bad outputs from bad prompts | Missing/wrong context | Unreliable agent behavior at scale |
| **Determinism** | Low | Medium | High (with computational sensors) |
| **Human Effort** | Per-interaction | Per-pipeline | Per-system (amortized) |

### Key Concept Disambiguation

![[image-20260406-064707.png]]

### Harness Engineering vs Multi-Agent Systems

|  |  |  |
|----|----|----|
| Dimension | Harness Engineering | Multi-Agent Systems |
| **What it is** | A discipline / design philosophy | An architecture pattern |
| **Scope** | Everything outside the model | How multiple models collaborate |
| **Relationship** | Harness Engineering **contains** multi-agent coordination | Multi-agent systems **need** harness engineering to work |
| **Example** | Tool permissions, memory, feedback loops | Planner → Generator → Evaluator |
| **Can exist alone?** | Yes (single-agent + harness) | Technically yes, but unreliable without harness |

![[image-20260406-064751.png]]

## Anthropic's Three-Agent Architecture

In March 2026, Anthropic published their [harness design for long-running apps](https://www.anthropic.com/engineering/harness-design-long-running-apps), revealing a GAN-inspired multi-agent architecture.

### Architecture Flow

### Cost & Performance Comparison

|                    |                        |                               |
|--------------------|------------------------|-------------------------------|
| Metric             | Solo Agent             | Full Harness (3-Agent)        |
| **Task**           | Retro 2D Game Maker    | Retro 2D Game Maker           |
| **Duration**       | 20 minutes             | 6 hours                       |
| **Cost**           | \$9                    | \$200                         |
| **Output Quality** | Broken, non-functional | Working game with AI features |
| **Model**          | Claude Opus 4.5        | Claude Opus 4.5               |

|                  |                                         |
|------------------|-----------------------------------------|
| Metric           | Full Harness (DAW App)                  |
| **Task**         | Browser-based Digital Audio Workstation |
| **Duration**     | ~3 hours 50 minutes                     |
| **Cost**         | \$124.70                                |
| **Planner Time** | 4.7 min (\$0.46)                        |
| **Build Rounds** | 3h 20m (\$113.85)                       |
| **QA Rounds**    | 25.2 min (\$10.39)                      |
| **Model**        | Claude Opus 4.6                         |

### Key Failures That Led to This Design

|  |  |  |
|----|----|----|
| Failure Pattern | What Happened | Harness Solution |
| Scope creep | Agent tried too many features at once | Planner breaks work into sprints |
| Premature "done" | Agent declared task complete prematurely | Evaluator independently verifies |
| Self-overestimation | Agent rated own work too highly | Separate evaluator agent (GAN-inspired) |

## Martin Fowler's Framework

Birgitta Böckeler (Thoughtworks) published a comprehensive [harness engineering framework](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html) on Martin Fowler's site.

### Guides vs Sensors

![[image-20260406-064943.png]]

### Two Modalities of Control

|  |  |  |  |
|----|----|----|----|
| Modality | Speed | Determinism | Examples |
| **Computational** | Milliseconds | Deterministic | Linters, type checkers, unit tests |
| **Inferential** | Seconds | Non-deterministic | LLM-based code review, semantic analysis |

### Three Regulation Categories

|  |  |  |
|----|----|----|
| Category | What It Regulates | Tools |
| **Maintainability** | Code quality | Linters, complexity analyzers, coverage tools |
| **Architecture Fitness** | System design | Fitness functions, module boundary enforcers |
| **Behavior** | Functional correctness | Specifications, test suites, manual review |

### Ashby's Law Applied

> "A regulator must have at least as much variety as the system it governs."

This means: to control an AI agent that can generate any code, you need a harness with equally broad coverage. Constraining the solution space (e.g., fixed tech stack) makes comprehensive harnesses achievable.

## Relationship to Multi-Agent Systems

### How They Connect

![[image-20260406-065032.png]]

### Multi-Agent Patterns Within Harness Engineering

|  |  |  |  |
|----|----|----|----|
| Pattern | Description | When to Use | Example |
| **Hub-and-Spoke** | Dispatcher routes to specialist agents | Complex tasks needing diverse skills | Claude Code's /scout + /team |
| **Pipeline** | Sequential handoff between agents | Tasks with clear phases | Planner → Generator → Evaluator |
| **Collaborative** | Agents share a task board | Parallel independent work | GoClaw Agent Teams |
| **Adversarial** | Generator vs Evaluator (GAN-style) | Quality-critical outputs | Anthropic's harness design |

## Architecture Diagrams

![[image-20260406-065121.png]]

### Claude Code's Five-Tier Memory Architecture

From the Claude Code source leak (March 31, 2026):

![[image-20260406-065200.png]]

### Evolution Timeline

![[image-20260406-065302.png]]

## Questions & Further Research

### Open Questions in Harness Engineering

|  |  |  |  |
|----|----|----|----|
| \# | Question | Current State | Suggested Research Direction |
| 1 | **How do you measure harness quality?** | No standard metrics exist | Develop a "Harness Coverage Score" analogous to code coverage — measuring what % of agent failure modes are addressed by guides/sensors |
| 2 | **When should harness complexity decrease?** | Anthropic noted sprints became removable with Opus 4.6 | Track which harness components become redundant as models improve; build adaptive harnesses that simplify themselves |
| 3 | **How do you prevent harness contradictions?** | As harnesses grow, rules can conflict | Research conflict detection algorithms for harness rules, similar to firewall rule analysis |
| 4 | **Can AI design its own harness?** | Stanford's Meta-Harness paper (arXiv:2603.28052) explores this | Pursue recursive harness optimization where AI observes its own failures and proposes harness improvements |
| 5 | **What is the ROI of harness investment?** |  | Build cost-quality models for different harness configurations per task type |
| 6 | **How does harness engineering apply to non-coding agents?** | Most examples are coding-focused | Extend frameworks to customer service, data analysis, creative tasks |
| 7 | **How do you handle "harnessability" of legacy systems?** | Fowler notes legacy systems need harnesses most but support them least | Research automated harnessability assessment tools |

> **Key Takeaway:**
>
> 1.  When models become commoditized, the competitive advantage shifts from "which model are you using?" to "how good is your harness?"
>
> 2.  Two teams using the exact same model can see task completion rates of **60% vs 98%** based entirely on harness quality.
>
> 3.  The model is table stakes. The harness is the moat.

## References

### Primary Source

- [Harness Engineering là gì? — Duy /zuey/ (Goon's Solo Playbook)](https://goonnguyen.substack.com/p/harness-engineering-la-gi) — April 5, 2026

### Foundational Works

- [My AI Adoption Journey — Mitchell Hashimoto](https://mitchellh.com/writing/my-ai-adoption-journey) — February 2026, origin of the term "Engineer the Harness"

- [SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering — Princeton NLP (NeurIPS 2024)](https://arxiv.org/abs/2405.15793) — The foundational research proving interface design \> model capability

- [Harness Design for Long-Running Apps — Anthropic Engineering](https://www.anthropic.com/engineering/harness-design-long-running-apps) — March 2026, three-agent harness architecture

### Framework & Analysis

- [Harness Engineering for Coding Agent Users — Birgitta Böckeler (Martin Fowler's site)](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html) — Comprehensive Guides & Sensors framework

- [Effective Context Engineering for AI Agents — Anthropic](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Context engineering as a subset of harness engineering

- [The Anatomy of an Agent Harness — LangChain Blog](https://blog.langchain.com/the-anatomy-of-an-agent-harness/)

### Industry Coverage

- [Harness Engineering: The Complete Guide — NxCode](https://www.nxcode.io/resources/news/harness-engineering-complete-guide-ai-agent-codex-2026)

- [What Is Harness Engineering? Introduction 2026](https://harnessengineering.academy/blog/what-is-harness-engineering-introduction-2026/)

- [What is AI Harness Engineering? — Mohit Sewak, Ph.D. (Medium)](https://medium.com/be-open/what-is-ai-harness-engineering-your-guide-to-controlling-autonomous-systems-30c9c8d2b489)

- [Harness Engineering: Uncovering What It Is — Data Science Dojo](https://datasciencedojo.com/blog/harness-engineering/)

- [The Third Evolution — Epsilla Blog](https://www.epsilla.com/blogs/harness-engineering-evolution-prompt-context-autonomous-agents)

- [Prompt vs Context vs Harness Engineering — PrivOcto (Medium)](https://medium.com/@server_62309/prompt-engineering-vs-context-engineering-vs-harness-engineering-whats-the-difference-in-2026-2883670f78f1)

- [From Prompt to Harness Engineering — DEV Community](https://dev.to/wonderlab/from-prompt-engineer-to-harness-engineer-three-evolutions-in-ai-collaboration-5bgp)

- [Harness Engineering: LLMs as the New OS — Decoding AI](https://www.decodingai.com/p/agentic-harness-engineering)

- [What Is an Agent Harness? — Salesforce](https://www.salesforce.com/agentforce/ai-agents/agent-harness/)

- [What Is an Agent Harness? — Parallel AI](https://parallel.ai/articles/what-is-an-agent-harness)

- [Harness Engineering — OpenAI](https://openai.com/index/harness-engineering/)

- [Effective Harnesses for Long-Running Agents — Anthropic](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)

### Academic

- Stanford Meta-Harness Paper — arXiv:2603.28052, April 2026 (AI self-optimizing harnesses)
