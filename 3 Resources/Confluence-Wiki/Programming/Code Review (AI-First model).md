---
ai_hash: f5aeff09a1070428
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.871
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/49441636397/Code+Review+AI-First+model
space: HACKA
status: reference
tags:
- confluence
- programming
- space/hacka
title: Code Review (AI-First model)
topic: programming
type: source
updated: 2026-05-25
---

# Code Review (AI-First model)

> [!info] Imported from Confluence
> Space **HACKA** · updated 2026-05-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/49441636397/Code+Review+AI-First+model)
> Relevance 0.871 · topic `programming`

Reason, description, why do we need to enhance our code review process

- PR is merged without approval → Lack of review Business/Technical solution

- Make the Code Review session is happened faster

Brain storming/Ideas/Goals

- All member involve

- Must involve AI

- Coding Convention

- Only merge when having at least 2 approval

- Review Solution → Agree or Not?

- Code Review event need to be happened more faster

- Who creating PR must let reviewers easy to understand

- Review prompt that lead to the PR’s solution?

- Business Logic

- PR should be small

- Prevent rework

- Define check list review so we can follow

- When Code Review event is happened?

  - When opening PR?

  - Periodically?

  - ASAP?

Agreement

- Review Business Logic.

  - Summarize business logic requirement.

  - Owner defines test cases in detail: from input → output

  - Meeting if it’s needed

- Review solution

  - Summary technical requirement

  - Summarize flow

  - Meeting if it’s needed

- All members: Optional

  - Active to review

- Must involve AI

  - Reuse Business Logic + Solution

  - Polaris

  - Code review agent/skill

  - Difference agents with PR

- PR must easy to understand

  - Small PR → feature branch

- Only merge when having at least 2 approval

- Convention

  - Common pattern

- When review

  - Periodically depend on person

  - Self organize

- **Define checklist**

  - **PR template**

    - **Having Business Logic / Solution**

    - **Test result**

    - **Small**

    - **Convention**

- **If PR is for a blocker → Skip unnecessary steps above.**

%% ai-graph-start %%

**Related notes:**
- [[Code review agreement]]
- [[02_30 Pull request and review code orally transmitted secrets]]
- [[Overview]]
- [[Investigate applying AI to security code review]]
- [[LLM-implementable plan exports must bundle unresolved review state with precedence rules]]

%% ai-graph-end %%