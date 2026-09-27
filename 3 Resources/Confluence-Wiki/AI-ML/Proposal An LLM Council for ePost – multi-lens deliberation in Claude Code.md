---
ai_hash: 3d1ace9d36e8f6ac
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 3
entities: []
relevance: 0.921
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49366532118/Proposal+An+LLM+Council+for+ePost+multi-lens+deliberation+in+Claude+Code
space: TK
status: reference
tags:
- confluence
- ai-ml
- space/tk
title: 'Proposal: An LLM Council for ePost – multi-lens deliberation in Claude Code'
topic: ai_ml
type: source
updated: 2026-04-30
---

# Proposal: An LLM Council for ePost – multi-lens deliberation in Claude Code

> [!info] Imported from Confluence
> Space **TK** · updated 2026-04-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49366532118/Proposal+An+LLM+Council+for+ePost+multi-lens+deliberation+in+Claude+Code)
> Relevance 0.921 · topic `ai_ml`

<div class="contentLayout2">

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

# Proposal: An LLM Council for ePost

> **Audience:** CTO, Product, Engineering Leads.  
> **Author:** Alvin Villanueva (PO, Team Kepler).  
> **Status:** Conceptual proposal. We are seeking approval to author one Claude-Code Skill and pilot it for two weeks.  
> **Goal in one line:** **better outputs from our Claude Code prompts on hard decisions, by replacing one agreeable voice with five contrasting lenses.**

## 1. The problem

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

A single LLM is **agreeable**. Ask Claude *"should we ship this?"* and you get five reasons to ship. Ask *"is this a bad idea?"* and you get five reasons it is. Same problem, different framing, opposite verdict. That is fine for writing emails. It is dangerous for product, architecture, and customer-facing decisions.

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

## 2. The pattern – cross-LLM → cross-lens

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

<a href="https://github.com/karpathy/llm-council" class="external-link" rel="nofollow">Andrej Karpathy's <em>LLM Council</em></a> replaces one model with several, has them peer-review each other anonymously, and lets a chair model synthesize. **Kepler's adaptation keeps the protocol but swaps the diversity source**: instead of five labs, we use **five thinking-lens sub-agents inside Claude Code**. Same anonymized peer review, same chair-with-the-power-to-dissent, same verdict structure – delivered inside the single-vendor stack we already run.

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

<div>

|  |  |
|----|----|
| **Karpathy's setup** | **Kepler's adaptation** |
| Four labs (OpenAI, Anthropic, Google, xAI), one prompt each. | One model (Claude), five thinking-lens prompts. |
| Diversity from training-data and alignment differences. | Diversity from prompt-defined lenses that contrast by design. |

</div>

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

The trade-off is honest: prompt-induced diversity is shallower than lab-diversity. We accept that ceiling because the protocol's other mechanics – lens contrast, anonymized peer review, a chair empowered to dissent – carry most of the value.

## 3. The five voices

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Lens</strong></p></th>
<th><p><strong>What they look for</strong></p></th>
<th><p><strong>Markdown Files</strong></p></th>
</tr>
&#10;<tr>
<td><p>The Contrarian</p></td>
<td><p>What is wrong, what will fail, the fatal flaw we are avoiding.</p></td>
<td><div id="expander-1129894407" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="c318e780-05d0-42d1-9f43-f32a91df9d03" data-macro-name="expand">
<div id="expander-control-1129894407" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">TheContrarian.md</span>
</div>
<div id="expander-content-1129894407" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="888311e2-ad37-4cab-a2a1-5428464c1df3" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>### The Contrarian
&gt; You are the Contrarian on a Kepler council.
&gt; Your thinking style: actively look for what is wrong, what is missing, what
&gt; will fail. Assume the idea has a fatal flaw and dig until you find it. If
&gt; everything looks solid, dig deeper. You are not a pessimist — you are the
&gt; friend who saves the user from a bad deal by asking the question they are
&gt; avoiding.
&gt;
&gt; Task classification: &lt;validation | problem-finding | solution-design&gt;
&gt; Length target: &lt;PER_ADVISOR_TARGET&gt; words
&gt; The framed question:
&gt; &lt;FRAMED_QUESTION&gt;
&gt;
&gt; Respond from your lens at roughly the length target. Cover the full
&gt; breadth of your lens. No preamble. Be direct and specific. Do not hedge.
&gt; Do not try to be balanced — the other advisors cover the angles you are
&gt; not. Substance over length: do not pad to hit the target, do not truncate
&gt; a real point to stay under it.</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td><p>The First Principles Thinker</p></td>
<td><p>The real question underneath the question. Strips away assumptions.</p></td>
<td><div id="expander-1802435760" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="f6d6fd86-8293-4ca5-9e2c-cb37d1863ec8" data-macro-name="expand">
<div id="expander-control-1802435760" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">TheFirstPrinciplesThinker.md</span>
</div>
<div id="expander-content-1802435760" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="fc7771e0-7480-4164-8403-e062cc5a0fb3" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>### The First Principles Thinker
&gt; You are the First Principles Thinker on a Kepler council.
&gt; Your thinking style: ignore the surface-level question and ask &quot;what are we
&gt; actually trying to solve here?&quot; Strip away assumptions. Rebuild the problem
&gt; from the ground up. Sometimes the most valuable output is &quot;you are asking
&gt; the wrong question entirely.&quot;
&gt;
&gt; Task classification: &lt;validation | problem-finding | solution-design&gt;
&gt; Length target: &lt;PER_ADVISOR_TARGET&gt; words
&gt; The framed question:
&gt; &lt;FRAMED_QUESTION&gt;
&gt;
&gt; Respond from your lens at roughly the length target. No preamble. Be
&gt; direct and specific. Substance over length.</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td><p>The Expansionist</p></td>
<td><p>Upside everyone else is missing. Adjacent opportunities.</p></td>
<td><div id="expander-606693404" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="8c24d29e-4194-4e20-b1af-11345c0cbc63" data-macro-name="expand">
<div id="expander-control-606693404" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">TheExpansionist.md</span>
</div>
<div id="expander-content-606693404" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="698869cc-bc4b-4e3c-af0f-5854333375d5" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>### The Expansionist
&gt; You are the Expansionist on a Kepler council.
&gt; Your thinking style: look for upside everyone else is missing. What could
&gt; be bigger? What adjacent opportunity is hiding? What is being undervalued?
&gt; You do not care about risk — that is the Contrarian&#39;s job. You care about
&gt; what happens if this works *better* than expected.
&gt;
&gt; Task classification: &lt;validation | problem-finding | solution-design&gt;
&gt; Length target: &lt;PER_ADVISOR_TARGET&gt; words
&gt; The framed question:
&gt; &lt;FRAMED_QUESTION&gt;
&gt;
&gt; Respond from your lens at roughly the length target. No preamble. Be
&gt; direct and specific. Substance over length.</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td><p>The Outsider</p></td>
<td><p>Has zero context about us. Catches the curse of knowledge.</p></td>
<td><div id="expander-507944063" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="6acfe9bd-9eaa-406e-a2cb-16e46c068acb" data-macro-name="expand">
<div id="expander-control-507944063" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">TheOutsider.md</span>
</div>
<div id="expander-content-507944063" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a9e0e745-6f07-4c6a-9ed3-1c4b9a761ba1" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>### The Outsider
&gt; You are the Outsider on a Kepler council.
&gt; Your thinking style: you have zero context about the user, ePost, or
&gt; Kepler. Respond purely to what is in front of you. You catch the curse of
&gt; knowledge — things obvious to insiders, confusing to a customer, a
&gt; regulator, or a new hire.
&gt;
&gt; Task classification: &lt;validation | problem-finding | solution-design&gt;
&gt; Length target: &lt;PER_ADVISOR_TARGET&gt; words
&gt; The framed question:
&gt; &lt;FRAMED_QUESTION&gt;
&gt;
&gt; Respond from your lens at roughly the length target. No preamble. Be
&gt; direct and specific. Substance over length.</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td><p>The Executor</p></td>
<td><p>Whether it can be done, and the fastest path on Monday morning.</p></td>
<td><div id="expander-927601298" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="bf0b670d-0076-4b14-ab0e-d7d15616aa1f" data-macro-name="expand">
<div id="expander-control-927601298" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">TheExecutor.md</span>
</div>
<div id="expander-content-927601298" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="3580225a-c55d-4af5-9e5d-4b67ff778334" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>### The Executor
&gt; You are the Executor on a Kepler council.
&gt; Your thinking style: you only care whether this can be done and the fastest
&gt; path to doing it. Ignore theory, strategy, big-picture thinking. Look at
&gt; every idea through &quot;OK, but what do you do Monday morning?&quot; If the idea
&gt; has no clear first step, say so.
&gt;
&gt; Task classification: &lt;validation | problem-finding | solution-design&gt;
&gt; Length target: &lt;PER_ADVISOR_TARGET&gt; words
&gt; The framed question:
&gt; &lt;FRAMED_QUESTION&gt;
&gt;
&gt; Respond from your lens at roughly the length target. No preamble. Be
&gt; direct and specific. Substance over length.</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

Three natural tensions: **Contrarian vs Expansionist** (downside vs upside), **First Principles vs Executor** (rethink vs do), with **the Outsider** in the middle keeping everyone honest.

## 4. How a session runs

The **orchestrator is the Claude Code main session itself**. When the user types a trigger phrase, the loaded Skill takes the wheel: it scans for context, frames the question, classifies it, sets length budgets, spawns five lens sub-agents in parallel, anonymizes their outputs, hands them to five separate reviewer sub-agents, dispatches to a chair sub-agent, writes two artefacts, opens the report in the browser, and hands control back to the user. All sub-agents are Claude — Sonnet for routine work, Opus for solution-design depth and chair synthesis.

The diagram below shows the routing. The six stages are described in detail in §4.1; model and budget mechanics in §4.2 and §4.3.


![[49366532118-diagram-export-28.4.2026-15_47_46-20260428-134746.png]]



  
‌

### 4.1 The six stages, in detail

</div>

</div>

</div>

<div class="columnLayout three-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

#### Stage 0 — Context scan

**Goal:** assemble the read-only context bundle that all downstream stages will share.

The orchestrator reads, in order:

1.  `CLAUDE.md` at the workspace root – Claude Code's auto-loaded project directive. Tells the session what the workspace is, the conventions, where to look first.

2.  `memory/` files indexed by `MEMORY.md` – the team's persistent profile (roles, preferences, prior decisions). Auto-loaded by Claude Code regardless of trigger.

3.  **User-attached material** – any file the user dropped into the prompt (Confluence page, briefing, transcript, screenshot). Read for this session only, never persisted.

4.  **Targeted workspace globs** – keywords lifted from the user's question drive `Glob` calls (e.g. `**/*sender*`, `**/*cache*`). The top 3-5 matches by relevance + recency are read, capped by a token budget.

5.  **Exclusion filter** – paths or filenames matching customer-name patterns or the folders `customers/`, `accounts/`, `crm/` are dropped before reading, per §6.2.

(When the wiki of §6 exists, Stage 0 will also search the vault by filename, full-text, and backlinks, and pull the 2-3 most relevant pages into the bundle. That is *next-step* scope, not pilot scope.)

**Output:** a context bundle handed to Stage 1, listed by file path so any later stage can re-open any source.

**Code as ground truth.** The bundle is the *starting* context, not a fence. Stages 2 and 4 sub-agents have full tool access and are expected to call `Read` / `Glob` / `Grep` against the codebase whenever they need to verify a claim. Stage 0 is the assembly line; verification happens later by the lenses themselves.

**Model:** orchestrator (Sonnet). Stage 0 is mostly tool calls, not reasoning — Sonnet is sufficient and cheap.

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

#### Stage 1 — Frame, classify, set budgets

**Goal:** turn the user's raw input into the four crisp inputs every lens will share.

The orchestrator produces:

- **The framed question** – the user's prompt rewritten without ambiguity, with the actual decision surfaced. *"Is now the right time to add a Sender-Service cache?"* becomes *"Decide whether to start solution S3 in Sprint 156, given Phase 2 is already in flight and the Quickschild ramp begins June."*

- **The classification** – one of `validation/decision`, `problem-finding`, `problem-analysis`, `solution-design`. Drives lens model assignment (§4.2) and length budgets (§4.3).

- **The length budgets** – per-advisor, per-review, chair-verdict word ranges. Derived from classification × question complexity × how much of the bundle is genuinely relevant.

- **The lens directives** – the per-lens role envelopes spawned in Stage 2.

**Model:** orchestrator (Sonnet). Clean rewriting + light judgement; Opus is overkill here.

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

#### Stage 2 — Convene the five lenses

**Goal:** five contrasting takes on the framed question, each grounded in code where it matters.

The orchestrator **spawns five sub-agents in parallel** via Claude Code's `Agent` tool. Each receives an identical envelope **except one variable: the lens persona**.

Each sub-agent gets:

- The framed question and classification (Stage 1).

- The shared context bundle (Stage 0).

- **The lens directive** – its persona, what it looks for, what it must avoid, the angle it is required to take. The five directives are defined in `SKILL.md`; the charters in §3 are their summary.

- **Discipline rules** – substance over length, name your assumptions, no flattery, no hedging, dissent if warranted, cite evidence.

- **Output format** – structured markdown with required sections: *Position*, *Key reasoning*, *Assumptions I'm making*, *What would change my mind*.

- The length budget for this lens.

**Each lens has full tool access.** Lenses are *expected* to call `Read` / `Glob` / `Grep` against the codebase to verify framing or extend the context bundle. The Stage-0 bundle is the starting context, not the verification ceiling.

**Model assignment per lens** (proposed default; full table in §4.2):

- **Sonnet** for validation / problem-finding lenses (Contrarian, Outsider).

- **Opus** for solution-design lenses (First Principles, Executor) when the task is solution-design class.

**Output:** five markdown documents, one per lens, each labelled with its lens name.

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

------------------------------------------------------------------------

</div>

</div>

</div>

<div class="columnLayout three-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

#### Stage 3 — Anonymize and peer-review

**Goal:** rank and critique the five lens outputs without anyone — including the chair downstream — knowing who wrote which paper.

The orchestrator does the anonymization itself, in three steps:

1.  **Collect** the five lens outputs.

2.  **Re-label** them as A through E using a **fresh random mapping per session** – Contrarian becomes C in one run, A in the next. The mapping is held by the orchestrator and only revealed in the post-session transcript (Stage 5).

3.  **Strip identifying voice** – any tell-tale phrasing (*"as the Executor would observe…"*) is rewritten to neutral voice. Persona-specific tics that survive into the body of an argument are normalised.

Then the orchestrator hands the five anonymized papers to **five fresh reviewer sub-agents** — *not* the same agents that wrote them. Two reasons:

- **Recognition risk.** A lens reading its own output, even anonymized, is biased toward it.

- **Perspective freshness.** Reviewers arrive cold and judge on reasoning quality alone, not on co-authoring the round.

Each reviewer (Sonnet, parallel) receives all five anonymized papers (A-E), the framed question, the classification, and a review directive: **rank by reasoning quality, not by which conclusion you prefer**; cite evidence; flag fatal flaws and missed considerations; identify which paper, if any, would change your own mind and why.

**Output:** five peer-review documents, each ranking and critiquing all five papers.

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

#### Stage 4 — Chair synthesis

**Goal:** one verdict that synthesizes the strongest reasoning across the council, names where the council clashes, and is empowered to dissent from the majority if the minority case is stronger.

A **separate chair sub-agent — not the orchestrator** — receives the framed question, the five anonymized lens papers, and the five peer reviews. Its directive: produce a single verdict; cite the strongest reasoning from any paper; surface explicit disagreement (a *"where the council clashes"* section is required); side with a dissenter when their reasoning is strongest.

The chair is the only role where Opus is non-negotiable in the proposed defaults — synthesis quality and the dissent-rationale judgement justify the premium.

**Output:** one chair verdict in markdown. The orchestrator's only remaining job is artefact writing.

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

#### Stage 5 — Artefacts

**Goal:** durable, scannable record of the session, and return control to the user.

The orchestrator (Sonnet) takes the framed question, the lens papers (de-anonymized for the transcript), the peer reviews, the chair verdict, and writes:

- `<workspace>/council/council-report-<timestamp>.html` – the visual verdict, opened in the browser as the session's final action. *"Where the council clashes"* sits above the verdict so the reader cannot skip past disagreement.

- `<workspace>/council/council-transcript-<timestamp>.md` – full audit trail: framed question, classification, the five papers, the anonymization mapping, the five reviews, the chair verdict.

- *(Future, once the wiki of §6 exists)* `<vault>/decisions/<date>-<topic>.md` – redacted decision log filed back into the vault, customer names replaced by scale-tier categories, backlinked to the pages Stage 0 consulted.

Control returns to the user.

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

------------------------------------------------------------------------

### 4.2 Model assignment

<div>

|  |  |  |  |
|----|----|----|----|
| Stage | Actor | Model assignment (proposed default) | Why |
| **0. Context scan** | Orchestrator | Sonnet | Mostly tool calls (Glob + Read); cheap. |
| **1. Frame + classify + budget** | Orchestrator | inherits | Clean rewriting + light judgement. |
| **2. Convene** (×5 parallel) | Lens sub-agents | Opus | Solution-design lenses need maximum analytical depth. |
| **3. Peer review** (×5 parallel) | Reviewer sub-agents | Sonnet | Structured comparison; faster and cheaper than Opus. |
| **4. Chair synthesis** | Chair sub-agent | **Opus** | Highest synthesis quality and the dissent-rationale judgement. |
| **5. Artefacts** | Orchestrator | inherits | File writes + browser open. |

</div>

This is a *proposed default* – the pilot will tell us whether Opus is worth its premium for the chair, or whether Sonnet suffices.

### 4.3 Adaptive length budgets

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

Rather than hard-code per-advisor word ranges in the Skill, the **orchestrator sets per-session length budgets in Stage 1**, based on three signals:

- **Task classification** – validation / decision, problem-finding, or solution-design.

- **Question complexity** – number of sub-questions, technical depth, breadth of components touched.

- **Context breadth** – how much of `CLAUDE.md` / `memory/` / attached files actually pertains.

Reasonable anchor targets the orchestrator starts from:

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

<div>

|  |  |  |  |
|----|----|----|----|
| Task | Per-advisor (Stage 2) | Per-review (Stage 3) | Chair verdict (Stage 4) |
| validation / decision | 150 – 300 words | 200 – 300 | 300 – 500 |
| problem-finding | 400 – 700 | 300 – 500 | 600 – 1 000 |
| solution-design | 800 – 1 500 (no upper limit) | 300 – 500 | 1 000 – 2 000 + |

</div>

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

The orchestrator may adjust per session: a trivial validation may need only 150 words per advisor, a complex multi-service solution-design with deep context may justify 2 500 + per advisor on the architectural lenses (Contrarian, Executor) and tighter budgets on the framing lenses (Outsider). **Discipline rule \#8 — "substance over length" — overrides any number.** The targets are budgets, not floors.

### 4.4 Trigger phrases

*"council this"*, *"war room this"*, *"pressure-test this"*, *"stress-test this"*, *"debate this"*.

## 5. The end product

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

**One markdown Skill file**, committed to a small dedicated Bitbucket repo, installed by every team member's Claude Code. No backend, no automation, no workflow integration. Each session writes two artefacts next to whatever we are working on:

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ba8fb091-f409-4302-b679-f3f791d43bf7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
~/.claude/skills/kepler-council/
└── SKILL.md                                   ← the Skill (committed, shared)

<wherever the user is working>/
└── council/
    ├── council-report-2026-05-12T1430.html    ← visual verdict, opened in browser
    └── council-transcript-2026-05-12T1430.md  ← full audit trail
```

</div>

</div>

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

The complete `SKILL.md` body, the per-session transcript template, the Bitbucket repo layout, and the install commands live on a dedicated child page so this proposal stays scannable: [**kepler-council – Skill source files**](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49366499345/kepler-council+Skill+source+files).

## 6. Getting the right context in front of the council

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

The council is only as good as the context the question lands with. Today, asking Claude Code a Kepler-specific technical question (*"why is search slow in luz-docs?"*) requires the user to know which files matter — which defeats the purpose when the question crosses repos or comes from a non-engineer.

<a href="https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f" class="external-link" rel="nofollow">Karpathy's <em>LLM Wiki</em> gist</a> proposes a clean answer: an **LLM-maintained markdown wiki**, browsed in **Obsidian**, sitting between raw sources and queries. Knowledge is compiled once into linked markdown pages and kept current — not re-derived from raw sources on every query.

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

### 6.1 What the Kepler wiki would contain

A small Obsidian vault with four page types. Each page is one markdown file, hand-edited or LLM-generated, linked to the others via Obsidian's `[[wikilink]]` syntax. A `CLAUDE.md` at the vault root describes the structure and conventions — Karpathy's "schema" layer.

<div>

|  |  |  |
|----|----|----|
| Page type | Examples | Where it comes from |
| **Service pages** | `luz-docs.md`, `luz-audit.md`, `luz-cache.md`, `luz-jsonstore.md` | Repo READMEs + recurring perf-relevant findings |
| **Topic pages** | `eArchive-performance.md`, `pricing-models.md`, `data-portability.md` | Cross-referenced summaries pulled from analyses and Jira |
| **Decision logs** | `decisions/2026-05-12-sender-cache.md` | Council verdicts, ADRs — written back from the council's Stage 5 |
| **Sprint retros** | `sprints/sprint-155.md`, ... | Retro notes |

</div>

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

### 6.2 Data boundary — what does **not** go into the wiki

The wiki indexes **product, topic, and decision** knowledge — content that is safe to share across the team and persist on every team-member's laptop. **Customer-specific material stays out of the vault**: customer letters, account financials, support tickets, briefings naming individual accounts. These stay in their authoritative systems — Confluence (with page-level access controls), the CRM, Jira tickets — where access can be revoked centrally.

The reason is simple: **a wiki is a compounding artefact.** Every laptop with the vault, every backup, every accidental commit becomes a copy. Customer content compounds the wrong way.

When a council session genuinely needs a customer dimension, the user **attaches the relevant Confluence page or briefing to that one prompt** — it informs the deliberation but is never copied into the vault. Three rules the Skill enforces in Stages 0 and 5:

1.  **Stage 0 ignores customer-named files** in any folder named `customers/`, `accounts/`, `crm/`, or matching a customer-name pattern.

2.  **Stage 5 redacts named accounts** when filing the verdict back into the vault — `Quickschild` becomes `scale-tier customer (~128 k documents)`, etc. The full transcript next to the working directory keeps the names; the wiki's decision log does not.

3.  **Customer documents attached for one session are read but never persisted** — they live only in the framed-question context, never written to disk by the Skill.

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

### 6.3 How a question travels through the wiki

When the user triggers the council, **Stage 0 of the protocol no longer reads only** `CLAUDE.md` and `memory/` — it also runs a search against the Obsidian vault and pulls the most relevant pages into the framed question.

The wiki is therefore both **input** to the council (retrieval at Stage 0) and **output sink** (the verdict at Stage 5). Every council session leaves the next one better-informed — without ever accumulating customer-specific content on disk.

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e5d814f2-83a1-4630-82a4-c5d4f8627802" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
User question  +  optionally-attached customer document (one-shot, never persisted)
    │
    ▼
Stage 0  →  search the vault (filename, full-text, backlinks)
                  │   (skipping any path or filename matching customer-name patterns)
                  ▼
            select the 2 – 3 most relevant pages
                  │
                  ▼
            inline their relevant sections + the attached document
            into the framed question
                  │
                  ▼
        Stages 2 – 4 deliberate with grounded, Kepler-specific context
                  │
                  ▼
        Stage 5 writes the verdict back into the vault as a decision log
        (with customer names redacted to scale-tier categories),
        backlinked to the pages it consulted — the wiki compounds.
```

</div>

</div>

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

### 6.4 Worked example

A user types:

> *"council this: is now the right time to add a Sender-Service cache (Phase 2 / Solution S3)?"*

Because the customer dimension matters here, the user also attaches the Quickschild prep Confluence page to the prompt as a one-shot context.

What happens:

1.  **Stage 0** searches the vault for `Sender-Service`, `cache`, `Phase 2`, `S3`. It hits two pages: `eArchive-performance.md` (the bottleneck analysis) and `luz-cache.md` (existing Redis infrastructure). The attached Quickschild page is read into the framed question alongside, but is **not** ingested into the vault.

2.  **Stages 2 – 4** deliberate with that grounded context. Every advisor sees the existing cache infrastructure, the Phase-2 plan, and the customer impact. The Outsider's "obvious to insiders" critique lands against the actual customer language we use in the briefing.

3.  **Stage 5** writes the chair's verdict to `<workspace>/council/council-transcript-2026-05-12T1430.md` (the full transcript, customer name intact) **and** files a summary at `<vault>/decisions/2026-05-12-sender-cache.md` — with the Quickschild reference redacted to *"scale-tier customer (~128 k documents, +25 k/year, sensitive to per-document unit economics)"*. Backlinks point to `eArchive-performance.md` and `luz-cache.md` only. Next time anyone asks about caching, those links resurface — without dragging the customer name into the searchable wiki.

### 6.5 Pilot scope reminder

**We are not proposing to build the wiki in this pilot.** The council pilot uses today's `CLAUDE.md` / `memory/` / attached-files mechanism. The wiki is the obvious next step **if** the council pilot succeeds — and the data boundary in §6.2 would be a load-bearing requirement of that follow-up proposal. The smallest first wiki would seed itself from `luz_docs` / `luz_audit` READMEs and recent sprint retros — strictly product, topic, and decision content; no customer files.

## 7. Three Kepler use cases

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

<div>

|  |  |  |
|----|----|----|
| Use case | Reference artefact | What the council adds |
| **Problem-finding** | Oncall report: *"eArchive feels slow for big customers"* | A ranked candidate-bottleneck list. The gap between five lenses is itself signal about where to dig. |
| **Problem-analysis** | The Quickschild customer letter | A briefing that surfaces the lock-in / unit-economics / churn subtext alongside the pricing surface. |
| **Solution-design** | L2 – pre-compute security-class codes | One recommended design **plus a rejected-alternatives list with reasons** – the artefact we never write today. |

</div>

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

## 8. When *not* to council

- One-right-answer questions (*"what does this MongoDB error mean?"*).

- Creation tasks (*"draft this email"*).

- Processing tasks (*"summarize this PR"*).

- Validation-seeking. **If we already know the answer and want a hug, the council will tell us what we do not want to hear. That is the entire point.**

Rule of thumb: *if being wrong is expensive, council it. Otherwise, do not.*

## 9. Risks and guardrails

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

<div>

|  |  |
|----|----|
| Risk | Mitigation |
| **Diversity is shallower than Karpathy's** – one model, five prompts; advisors share base biases. | Frame the council as "structured perspective-taking", not cross-lab peer review. For the highest stakes, pair the verdict with a human reviewer chosen because they would disagree. |
| **Sycophancy in the chair** – same model assessing its own outputs may smooth, not challenge. | Anonymization at peer review (Stage 3) is critical. The chair is explicitly empowered – and required – to side with a dissenter when the reasoning is strongest. |
| **Token cost** – ≈ 6× a single-model query, all within our existing Claude Code subscription. | Reserve council usage for hard decisions. The Skill's triggers refuse trivial questions. |
| **Over-trust** – a polished verdict can short-circuit human judgement. | The HTML report makes disagreement visible. Reviewers must read the *"Where the council clashes"* section, not skip to the verdict. |

</div>

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

## 10. Pilot

Three council runs against three already-completed Kepler artefacts where we know the right answer:

1.  **Problem-finding** – feed the council the *raw* eArchive customer complaints from before the analysis was written. Compare to Liem's actual eight-bottleneck breakdown.

2.  **Problem-analysis** – feed the council the *raw* Quickschild letter. Compare to the v4 prep page the PO produced manually.

3.  **Solution-design** – feed the council the *problem statement* of L2. Compare its design and rejected-alternatives list to the design Liem actually shipped.

**Setup.** Create the `kepler-council` repo in `axonivy-prod`, commit `SKILL.md` (see [source files](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49366499345/kepler-council+Skill+source+files)), install per the PO and one engineer.

**Duration.** Two weeks. ≈ 1 PO + 1 engineer at part-time.

**Success criteria.** ≥ 2 of 3 cases: the council surfaces material findings that match or extend the human-produced output. The transcript is readable and useful to a reviewer who didn't watch it happen. We have an opinion on which advisor most often delivers the highest-value insight, **and on whether Opus is worth the premium for the chair role.**

## 11. What I am asking the CTO to decide

1.  **Approve creating the** `kepler-council` repo in `axonivy-prod` and committing the Skill (full content on the [source-files child page](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49366499345/kepler-council+Skill+source+files)), then running the two-week pilot above.

2.  **Nominate a reviewer** for the end-of-week-2 readout – someone willing to disagree with the council.

Because everything runs inside Claude Code, **no separate budget, no DPA review, and no third-party data-handling boundary** is required.

## 12. References

- <a href="https://github.com/karpathy/llm-council" class="external-link" rel="nofollow">GitHub: karpathy/llm-council</a> – the original cross-LLM council pattern, which this proposal adapts to cross-lens.

- <a href="https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f" class="external-link" rel="nofollow">Gist: karpathy/LLM Wiki</a> – the personal-wiki / Obsidian pattern referenced in §6 as the next step for context-loading.

- [kepler-council – Skill source files](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49366499345/kepler-council+Skill+source+files) – child page with the full `SKILL.md`, transcript template, repo layout, and install commands.

- [Performance Analysis and Proposed Solutions](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49317347331/Performance+Analysis+and+Proposed+Solutions) – the eArchive case used as a reference example.

- [Customer Questions eArchive – Proposed Answers (EN)](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49367384073/Customer+Questions+eArchive+Proposed+Answers+EN) – the Quickschild case.

- [Pre-compute Security Class Code](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49357979662/Pre-compute+Security+Class+Code+-+Eliminate+Lookup+Query) – the L2 case.

</div>

</div>

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Manufacture structured disagreement when real model independence is unavailable]]
- [[A coding-agent prompt needs codebase anchors and stated house style]]
- [[Flow View - V2]]
- [[Agent Loop 3 - Test-Plan Definition]]
- [[Agent Loop 1 - Knowledge Gathering - v2]]

%% ai-graph-end %%