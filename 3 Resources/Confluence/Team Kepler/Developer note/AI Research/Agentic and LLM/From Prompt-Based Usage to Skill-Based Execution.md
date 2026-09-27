---
title: "From Prompt-Based Usage to Skill-Based Execution"
created: 2026-03-19
updated: 2026-03-19
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49250435164/From+Prompt-Based+Usage+to+Skill-Based+Execution
confluence_id: "49250435164"
confluence_path: "Team Kepler > Developer note > AI Research > Agentic and LLM"
tags: [confluence, ai-agents, prompt-engineering, search]
---

# From Prompt-Based Usage to Skill-Based Execution

*Confluence source · Team Kepler › Developer note › AI Research › Agentic and LLM · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49250435164/From+Prompt-Based+Usage+to+Skill-Based+Execution) · updated 2026-03-19*

## 1. The Core Mindset: Skills Are Not "Prompt Tricks" — They Are Operational Assets

Most people currently use AI in a cycle like this: ask → receive an answer → tweak the prompt → try again. This approach works well for short, isolated, low-risk tasks.

But when work begins to involve multiple steps, multiple data sources, multiple correctness criteria, reusability requirements, validation requirements, and team scalability — prompts alone are no longer sufficient.

At that point, what you need to build is not a "better prompt," but a structured skill system — one where the agent doesn't merely read instructions, but also understands context, retrieves the right knowledge, executes the right workflow, uses the right scripts, validates its output, and self-corrects after each failure.

In short:

- A **prompt** is a temporary instruction.

- A **skill** is an encapsulated capability.

- A **skill system** is the operational memory of an individual or an organization.

------------------------------------------------------------------------

## 2. The Foundation Model: 5 Layers of a Complete Skill System

The following five-layer model is proposed for individual and project application:

### Layer 1: Intent Layer — What Needs to Be Achieved

This layer defines the problem, what constitutes a completed output, the correctness criteria, and the scope of permissible processing. Without clarity here, the AI will generate outputs that appear correct but fail to address the actual objective.

**Examples:**

- Write a training proposal for an enterprise client

- Create a course outline for a sales team

- Analyze AI use cases for an HR department

- Draft a landing page outline for an AI product

- Generate a file upload module with test coverage

*The Intent Layer answers: What are we trying to accomplish, and by what standard?*

### Layer 2: Knowledge Layer — The Knowledge Required to Act

This layer holds domain knowledge, business rules, best practices, style guides, API references, internal frameworks, brand voice, and standard templates.

**For individuals:**

- Your LinkedIn writing style

- Your enterprise AI training framework

- Your standard enterprise proposal template

- Your system prompt construction checklist

- Your AI use case evaluation criteria

**For projects:**

- Product documentation

- Database documentation

- API docs

- Coding standards

- Release processes

- System architecture diagrams

*The Knowledge Layer answers: What does one need to know in order to act correctly?*

### Layer 3: Execution Layer — How to Execute

This is the layer most commonly overlooked. A skill should not merely describe "how to do something" — it should contain directly usable artifacts that the agent can act on:

- Scripts

- Commands

- Workflows

- Sample queries

- File templates

- Automation routines

- Scaffold code

- Operational checklists

**Examples:**

- A script to generate project folder structure

- A PRD template

- Deployment commands

- SQL queries for data validation

- A prompt template for generating case studies

- A Python script to parse CSV files

- A workshop agenda template

*The Execution Layer answers: How is this done, in executable, step-by-step form?*

### Layer 4: Verification Layer — Validate Before Completion

This is the most critical layer. Many AI systems fail not because they cannot generate output, but because there is no testing, cross-referencing, simulation, real-world validation, or clear pass/fail criteria.

A robust skill must include expected outputs, test cases, anti-patterns, review checklists, simulation steps, sanity checks, and expected-vs-actual comparisons.

**Examples:**

- Does the proposal include all mandatory sections?

- Does the code pass its tests?

- Is the training content appropriate for a non-technical audience?

- Does the prompt produce output in the correct format?

- Can the agent handle edge cases with incomplete input?

*The Verification Layer answers: How do we know that what was produced is correct, usable, and trustworthy?*

### Layer 5: Evolution Layer — Learn from Failures and Upgrade the Skill

Skills are not static documentation. Every instance of an error, hallucination, incorrect output format, missed step, misunderstood business rule, edge case failure, or excessive manual correction represents data for updating the skill.

**The Evolution Layer includes:**

- `gotchas.md`

- Failure cases

- Edge cases

- Lessons learned

- Changelog

- Version history

- Improvement notes

*The Evolution Layer answers: After each use, where does the skill system become smarter?*

------------------------------------------------------------------------

## 3. A Standard Framework: 8 Skill Types to Have

To avoid creating skills arbitrarily, skills should be organized by functional role.

### 3.1 Knowledge Skills

Provide foundational domain knowledge. *Examples:* `enterprise-unicharm-overview`, `brand-voice-profile`, `genai-training-framework`, `enterprise-proposal-standards` *Best suited for:* content writing, analysis, consulting, training, strategic design.

### 3.2 Verification Skills

Validate outputs against defined criteria. *Examples:* `verify-proposal-completeness`, `verify-training-outline-fit-nontech`, `verify-code-quality`, `verify-slide-structure`, `verify-agent-output-format` *This skill type creates the greatest distinction between "AI that responds well" and "AI that operates reliably."*

### 3.3 Data Skills

Read, process, and analyze data. *Examples:* `analyze-excel-sales-report`, `clean-customer-list`, `extract-insights-from-survey`, `compare-training-needs-by-department`

### 3.4 Automation Skills

Execute repetitive, multi-step workflows. *Examples:* `create-proposal-folder-structure`, `generate-client-kickoff-pack`, `summarize-meeting-and-create-actions`, `publish-content-multi-platform`

### 3.5 Scaffolding Skills

Generate initial structural frameworks. *Examples:* `scaffold-laravel-project`, `scaffold-training-proposal`, `scaffold-course-outline`, `scaffold-agent-skill-folder`

### 3.6 Review Skills

Critique and elevate quality. *Examples:* `review-business-logic`, `review-strategy-deck`, `review-copywriting-clarity`, `review-system-prompt`

### 3.7 Runbook Skills

Handle operational scenarios with defined procedures. *Examples:* `runbook-client-onboarding`, `runbook-bug-triage`, `runbook-training-delivery-day`, `runbook-demo-preparation`

### 3.8 Infra / CI / Delivery Skills

Support technical deployment and system quality control. *Examples:* `deploy-vercel-app`, `run-pre-release-checks`, `setup-firebase-auth`, `ci-checklist-agent-project`

------------------------------------------------------------------------

## 4. Golden Principles of Skill Design

### Principle 1: One Skill = One Responsibility

Do not pack multiple concerns into a single skill.

❌ Wrong: `skill_write_check_post_facebook_create_image`

✅ Correct:

- `write-facebook-post`

- `verify-facebook-post`

- `generate-cover-image-brief`

- `publish-social-content`

The more single-purpose a skill, the more accurately the agent selects and applies it.

### Principle 2: A Skill Is a Folder, Not a Single File

A skill folder may contain instructions, examples, scripts, templates, test cases, sample data, and a changelog.

Context quality does not come from a long block of text — it comes from a clear structure.

```
skills/
  write-enterprise-proposal/
    SKILL.md
    templates/
      proposal-outline.md
      pricing-table.md
    examples/
      sample-proposal-1.md
    verification/
      checklist.md
    assets/
      company-profile-summary.md
    changelog.md
```

A skill structured this way is far more powerful than a 500-line prompt.

### Principle 3: Always Include Verification

Without verification, a skill is merely a generator. For a skill to become a trustworthy execution capability, it must include a checklist, acceptance criteria, test cases, known failure patterns, and validation methods.

### Principle 4: Structure Over Rigid Control

A common mistake is attempting to micromanage the agent at every step:

- Step 1: do this

- Step 2: do that

- Step 3: do not deviate

- Step 4: this exact sentence is required

- Step 5: no variation allowed

This approach overwhelms the agent, reduces flexibility, and leads to poor handling of variations.

Instead: define the objective, provide accurate context, supply the right tools, define verification standards, and give the agent sufficient space to adapt.

**The right mindset:**

> Don't try to control every move the AI makes. Design an environment in which the AI performs well.

------------------------------------------------------------------------

## 5. Personal Skill OS: An Operational Skill System for Individuals

For personal use, the following six skill clusters are proposed:

**Cluster 1 — Thinking Skills:** Strategic thinking, analysis, and writing *Examples:* `think-strategically`, `break-down-complex-problem`, `synthesize-long-report`, `write-in-my-voice`, `build-framework-from-ideas`

**Cluster 2 — Content Skills:** Content creation across channels *Examples:* `write-facebook-post`, `write-linkedin-article`, `write-training-proposal-summary`, `write-seo-article`, `create-video-script-shortform`

**Cluster 3 — Teaching Skills:** Training design and curriculum development *Examples:* `design-nontech-ai-training`, `build-case-study-by-department`, `create-hands-on-exercises`, `adapt-content-for-executives`, `generate-training-assessment`

**Cluster 4 — Consulting Skills:** Advisory, research, and needs analysis *Examples:* `diagnose-enterprise-ai-readiness`, `build-ai-usecase-map`, `design-interview-question-bank`, `write-prd-for-client`, `design-ai-roadmap`

**Cluster 5 — Build Skills:** Product, prototype, and system development *Examples:* `scaffold-agent-project`, `build-laravel-module`, `create-database-schema`, `create-n8n-workflow-spec`, `setup-rag-structure`

**Cluster 6 — Operating Skills:** Personal and team operations *Examples:* `meeting-note-to-actions`, `weekly-review-and-planning`, `create-client-delivery-checklist`, `update-knowledge-from-lessons`, `postmortem-after-project`

------------------------------------------------------------------------

## 6. Project Skill OS: A Skill System for Each Project

When applied to a project, treat each project as a mini operating system.

**Proposed structure:**

```
project/
  context/
    business_goal.md
    stakeholders.md
    scope.md
    terminology.md
  skills/
    knowledge/
    verification/
    automation/
    scaffolding/
    review/
    runbooks/
    infra/
  templates/
  data/
  scripts/
  outputs/
  logs/
  lessons/
```

**Purpose of each area:**

- `context/` — helps the agent understand the problem space

- `skills/` — encapsulated capabilities

- `templates/` — document, code, and output templates

- `data/` — sample data, schemas, mappings

- `scripts/` — executable components

- `outputs/` — results storage

- `logs/` — execution tracking

- `lessons/` — captured learnings for system improvement

------------------------------------------------------------------------

## 7. A Practical Implementation Methodology: 7-Step Process

1.  **Identify high-value, repeatable tasks** — tasks you perform frequently, that are error-prone, time-consuming, structurally consistent, and standardizable.

2.  **Decompose each task into independent units** — instead of a single "write proposal" skill, break it into `analyze-client-need`, `generate-proposal-outline`, `write-proposal-executive-summary`, `build-pricing-options`, `verify-proposal-completeness`.

3.  **Package each skill as a folder** — every skill should contain at minimum: `SKILL.md`, `examples/`, `templates/`, `verification/`, `changelog.md`. Technical skills should also include `scripts/` and `test-data/`.

4.  **Write** `SKILL.md` **according to a standard structure** — including: Purpose, When to Use, Required Inputs, Expected Output, Execution Approach, Quality Criteria, Verification, Edge Cases, Examples, and Changelog.

5.  **Deploy and test on 5–10 real problems** — track which skills save the most time, which produce suboptimal output, and where errors recur.

6.  **Document gotchas and update the version** — update the skill immediately following each failure.

------------------------------------------------------------------------

## 8. Standard `SKILL.md` Template

```
# SKILL: write-enterprise-proposal

## Purpose
Produce a professional enterprise-grade training or consulting proposal
with clear logic and relevance for executive decision-makers.

## Use When
- Writing a proposal for an enterprise client
- Standardizing a proposal structure
- Maintaining a professional, strategic tone

## Required Inputs
- Client name
- Context and business need
- Target audience
- Program objectives
- Estimated duration
- Specific requirements

## Expected Output
- A fully structured proposal
- Clear, persuasive language, avoiding excessive academic tone
- Logical flow: context → problem → objectives → program → methodology → expected outcomes

## Execution Approach
1. Analyze the business context
2. Group needs into 3–5 primary themes
3. Design the program to match the audience
4. Write the proposal according to the standard structure
5. Review for clarity, feasibility, and persuasiveness

## Quality Criteria
- No unnecessary elaboration
- Contextually appropriate for the industry
- Suitable for non-technical audiences when specified
- Concrete, measurable outcomes
- No generic or vague language

## Verification
- Are all mandatory sections present?
- Is each section tied to a real business need?
- Does the proposal have a logical implementation rationale?
- Is any content overly generic?

## Edge Cases
- Client describes needs vaguely
- Scope is too broad for the available timeframe
- Target audience is highly heterogeneous
- Client wants "hands-on" content but lacks real data

## References
- `templates/proposal-outline.md`
- `examples/sample-proposal-b2b.md`
- `verification/checklist.md`

## Changelog
- v1.2: Added departmental outcome section
- v1.3: Added non-technical audience fit check
```

------------------------------------------------------------------------

## 9. The Verification Mechanism: A Mandatory Component of Every Skill

The proposed **4C Verification Framework:**

- **Correctness** — Is it accurate? Correct logic, facts, process, and format?

- **Completeness** — Is it complete? All sections, inputs, and steps included?

- **Context-Fit** — Is it contextually appropriate? Right industry, audience, objective, and technical level?

- **Consequence** — Is it safe to use in production? If sent to a client, would it cause issues? If the script runs, could it introduce errors? If handed to the team, could it be misunderstood?

Every skill should close with the question:

> *If this output were used immediately in a real-world scenario, what is the highest-risk failure mode?*

This is how output quality is elevated from "seems fine" to "reliable enough to use."

------------------------------------------------------------------------

## 10. Customizing This Framework for Your Personal Goals

Based on strategic direction, the framework can be customized into four primary tracks:

- **Track 1 — AI Strategy & Writing:** Translating research into business insight, writing in your own voice, building frameworks from research.

- **Track 2 — Enterprise Training:** Designing programs, writing proposals, building department-specific use cases, adapting content for non-technical audiences.

- **Track 3 — Products, Agents & Projects:** Scaffolding agent skill systems, designing system prompts, building RAG structures, verifying agent behavior, documenting failure lessons.

- **Track 4 — Business Operations & Delivery:** Converting client briefs to PRDs, turning meeting notes into action plans, diagnosing AI readiness, generating proposal follow-up communications.

|  |  |
|----|----|
| Phase | Action |
| Phase 1 | Catalog your 10 most frequently repeated tasks |
| Phase 2 | Standardize your first 5 skills (2 content, 2 training/consulting, 1 verification) |
| Phase 3 | Attach templates, examples, and checklists to each skill |
| Phase 4 | Deploy on 5–10 real problems; track performance and failure patterns |
| Phase 5 | Document gotchas and release updated skill versions |

------------------------------------------------------------------------

## 11. The Operational Formula

```
SCOPE → SKILL → EXECUTE → VERIFY → EVOLVE
```

- **SCOPE** — Define the problem, objective, and expected output

- **SKILL** — Select or create the appropriate module

- **EXECUTE** — Let the agent act with the right knowledge, tools, and workflow

- **VERIFY** — Validate using checklists, tests, and comparisons

- **EVOLVE** — Update the skill based on failures and edge cases

This loop is what generates compounding capability over time. A well-built skill system expands what you — and your organization — can reliably accomplish.
