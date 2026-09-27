---
ai_hash: e3484415efaa31bd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Harvest training labels as a side effect of the user's own goal
- Training data
- product AI
- labellers
- customer's private mail
- documents
- messages
- users
- Implicit annotation
- Explicit opt-in gamification
- app
- title of a document
- system
- labelled training example
- bounding-box-level positional data
- text label
- simple questions
- mail
- badges
- levels
- implicit loop
- user's incentive
- label
- bootstrapping a new skill
- real data
- research
- raw data
- user feedback
- problems
- Raw format
- Organised into logical collections
- model training
- reproducible
- Event-triggered ETL
- registered script
- specific event
- target system
- deployable data-extraction script
- data-exfiltration primitive
- approval and signing flow
- attacker
- careless engineer
- ePost AI Solution Concept
- AI
- Confluence
- Version the whole retrieval pipeline, not just the model
source: 'Confluence: ePost AI Solution Concept (AI)'
status: seedling
tags:
- training-data
- annotation
- privacy
- crowdsourcing
- product-design
- confluence-distilled
title: Harvest training labels as a side effect of the user's own goal
type: concept
---

# Harvest training labels as a side effect of the user's own goal

Training data for a product AI usually cannot be outsourced to labellers, because the data is the customer's private mail, documents, or messages. The resolution: **let users annotate their own data as a side effect of doing something they already wanted to do.** The labels are produced, and the data never leaves their control.

Two contribution modes, and the first is the important one:

**Implicit — annotation as a by-product of the user's own goal.** The app frames the request as help *for the user*, and the label falls out.

> The app tells the user it could not identify the **title** of a document. If the user wants, it guides them to select the title in the document. Afterwards the document has a title — **and the app knows its exact location on the page.**

The user got a titled document, which is what they wanted. The system got a labelled training example *with bounding-box-level positional data*, which is far richer than a text label alone and would have been expensive to buy.

**Explicit — opt-in gamification.** Users who want to help more receive simple questions about their mail and earn badges and levels. Strictly opt-in, and potentially a separate app.

> [!tip] Design the implicit loop so the user's incentive and the label are the same artefact
> The reason this works is that "I want my document titled" and "the model needs a title label" are satisfied by one action. When those diverge — when you are asking the user to do work purely for you — you are back to explicit contribution, which needs consent and a reward.

**The uncomfortable part the concept states plainly:** bootstrapping a *new* skill still needs access to real data for research. You must be able to explore, examine and visualise data to specify requirements at all, and you need raw data alongside user feedback to diagnose problems. The stated requirements:

- **Raw format** for research (a vectorised representation may come later).
- **Organised into logical collections**, so model training is **reproducible** — the same discipline as [[Version the whole retrieval pipeline, not just the model]].
- **Event-triggered ETL**: a registered script fires on a specific event (once on login, on update of field *x*) and extracts, transforms and loads to a target system — e.g. *if the user changes the title, send that page with its annotated title*.

> [!warning] A deployable data-extraction script is a data-exfiltration primitive
> The concept requires that deploying such a script be **secured by an approval and signing flow** — and that is not bureaucracy. A registered, event-triggered script with access to raw customer data is exactly the capability an attacker or a careless engineer would want. Approval plus signing means no script runs that someone did not review and that cannot be attributed. Design that control at the same time as the feature, not after.

Source: [[ePost AI Solution Concept]] (AI, Confluence).

## Related

- [[Version the whole retrieval pipeline, not just the model]]

%% ai-graph-start %%

**Related notes:**
- [[ePost AI Solution Concept]]

**Relations:**
- Harvest training labels as a side effect of the user's own goal — *is a resolution for* — Training data
- Training data — *for* — product AI
- Training data — *cannot be outsourced to* — labellers
- Training data — *includes* — customer's private mail
- Training data — *includes* — documents
- Training data — *includes* — messages
- Harvest training labels as a side effect of the user's own goal — *involves* — users
- users — *annotate* — their own data
- Implicit annotation — *is a contribution mode of* — Harvest training labels as a side effect of the user's own goal
- Explicit opt-in gamification — *is a contribution mode of* — Harvest training labels as a side effect of the user's own goal
- Implicit annotation — *is* — annotation as a by-product of the user's own goal
- app — *guides user to select* — title of a document
- system — *gets* — labelled training example
- labelled training example — *includes* — bounding-box-level positional data
- bounding-box-level positional data — *is richer than* — text label
- Explicit opt-in gamification — *involves* — simple questions
- simple questions — *about* — mail
- Explicit opt-in gamification — *can earn* — badges
- Explicit opt-in gamification — *can earn* — levels
- implicit loop — *aligns* — user's incentive
- implicit loop — *aligns* — label
- user's incentive — *and label are* — the same artefact
- bootstrapping a new skill — *needs* — real data
- real data — *for* — research
- bootstrapping a new skill — *requires* — Raw format
- bootstrapping a new skill — *requires* — Organised into logical collections
- bootstrapping a new skill — *requires* — Event-triggered ETL
- raw data — *and user feedback diagnose* — problems
- Organised into logical collections — *enables* — reproducible model training
- model training — *is reproducible like* — Version the whole retrieval pipeline, not just the model
- Event-triggered ETL — *uses* — registered script
- registered script — *fires on* — specific event
- registered script — *extracts, transforms and loads to* — target system
- deployable data-extraction script — *is a* — data-exfiltration primitive
- deployable data-extraction script — *requires* — approval and signing flow
- approval and signing flow — *prevents misuse by* — attacker
- approval and signing flow — *prevents misuse by* — careless engineer
- ePost AI Solution Concept — *is the source for* — Harvest training labels as a side effect of the user's own goal
- ePost AI Solution Concept — *is about* — AI
- ePost AI Solution Concept — *is related to* — Confluence
- Harvest training labels as a side effect of the user's own goal — *is related to* — Version the whole retrieval pipeline, not just the model

%% ai-graph-end %%