---
ai_hash: 9442fefffc2b44c9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- beforeafterai
- Polaris QA workflow map
- beforeafterai.excalidraw
- ai-agentic-framework/docs
- QA lifecycle map
- Test AI Agent
- Ops
- Report AI Agent
- KNOWLEDGE
- PLAN
- EXECUTE
- EVALUATE
- REPORT
- TRIAGE
- Knowledge gathering
- Knowledge refinement
- Test Plan definition
- Test Plan implement
- Test execution
- Test Evaluation definition
- Test Evaluation support
- Test Completion Report
- Test triage
- Test triage support
- interrogate-business
- interrogate-technical
- graphify-investigate
- interrogate-qa
- review-testability
- story-to-bdd-scenarios
- write-acceptance-tests
- implement-bdd-steps
- luz-docs-integration-test
- playwright-klara-earchive
- google-skill-gke-monitor
- luz-skill-flow-logs
- write-test-completion-report
- qa-html-before-delivery
- grounded-bug-report
- Polaris Memory Bank
- Jira
- Xray
- Confluence
- Agent Memory
- Git
- GCS
- evidence
- Bitbucket PR
- Bitbucket merge
- nightly-cron
- Cloud Build
- LUZ-159671 test-agent
- GCS-markdown memory bank
- vinnstack SKILL.md convention
- Knowledge-Gathering loop is a bounded frontier crawl with a verify edge
- Polaris AI skills
- Data store
- Trigger
- Phase
- Step
- Skill
source: ai-agentic-framework/docs/beforeafterai.excalidraw — session 2026-08-27
status: seedling
tags:
- qa-workflow
- polaris
- skills
- agents
- luz-159471
title: beforeafterai — the 10-step Polaris QA workflow map
type: model
---

# beforeafterai — the 10-step Polaris QA workflow map

The `beforeafterai.excalidraw` (in `ai-agentic-framework/docs`) is the canonical **QA lifecycle map**: 10 steps, grouped into 6 phases, owned by 3 agents, each step executed by concrete Polaris AI **skills** (purple in the diagram).

**Phases → owning agent:** KNOWLEDGE, PLAN, EXECUTE → **Test AI Agent** · EVALUATE → Test + **Ops** · REPORT → **Report AI Agent** · TRIAGE → Report + Ops.

**The 10 steps → skills:**
1. Knowledge gathering — interrogate-business, interrogate-technical, graphify-investigate  (→ Polaris Memory Bank)
2. Knowledge refinement — interrogate-qa
3. Test Plan definition — interrogate-qa, review-testability
4. Test Plan implement — story-to-bdd-scenarios, write-acceptance-tests
5. Test execution — implement-bdd-steps, luz-docs-integration-test, playwright-klara-earchive
6. Test Evaluation definition — review-testability
7. Test Evaluation support (monitoring) — google-skill-gke-monitor, luz-skill-flow-logs
8. Test Completion Report — write-test-completion-report, qa-html-before-delivery
9. Test triage — grounded-bug-report
10. Test triage support — grounded-bug-report

**Data stores:** Jira/Xray, Confluence, **Polaris Memory Bank** (+ Agent Memory), Git, GCS (evidence). **Triggers:** Bitbucket PR/merge/nightly-cron → Cloud Build. The LUZ-159671 test-agent maps to **Step 1 (KNOWLEDGE)**; its GCS-markdown memory bank implements the Polaris Memory Bank.

## Related

- [[vinnstack SKILL.md convention]]
- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]

%% ai-graph-start %%

**Related notes:**
- [[Testing Agent workflow step to AI Skill mapping]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[Ground-then-refine gathering grounds, refinement interprets and confirms]]
- [[luz_docs_integration_test has its own AI-driven BDD pipeline (generate, implement, PR agents)]]
- [[Agent skeleton = Instruction + Skills-Resources + Tools + Context]]

**Relations:**
- beforeafterai — *IS_A* — Polaris QA workflow map
- beforeafterai.excalidraw — *IS_A* — QA lifecycle map
- beforeafterai.excalidraw — *LOCATED_IN* — ai-agentic-framework/docs
- QA lifecycle map — *HAS_STEPS_COUNT* — 10
- QA lifecycle map — *HAS_PHASES_COUNT* — 6
- QA lifecycle map — *OWNED_BY_AGENTS_COUNT* — 3
- KNOWLEDGE — *IS_A* — Phase
- PLAN — *IS_A* — Phase
- EXECUTE — *IS_A* — Phase
- EVALUATE — *IS_A* — Phase
- REPORT — *IS_A* — Phase
- TRIAGE — *IS_A* — Phase
- KNOWLEDGE — *OWNED_BY* — Test AI Agent
- PLAN — *OWNED_BY* — Test AI Agent
- EXECUTE — *OWNED_BY* — Test AI Agent
- EVALUATE — *OWNED_BY* — Test AI Agent
- EVALUATE — *OWNED_BY* — Ops
- REPORT — *OWNED_BY* — Report AI Agent
- TRIAGE — *OWNED_BY* — Report AI Agent
- TRIAGE — *OWNED_BY* — Ops
- Knowledge gathering — *IS_A* — Step
- Knowledge refinement — *IS_A* — Step
- Test Plan definition — *IS_A* — Step
- Test Plan implement — *IS_A* — Step
- Test execution — *IS_A* — Step
- Test Evaluation definition — *IS_A* — Step
- Test Evaluation support — *IS_A* — Step
- Test Completion Report — *IS_A* — Step
- Test triage — *IS_A* — Step
- Test triage support — *IS_A* — Step
- Knowledge gathering — *IS_STEP_NUMBER* — 1
- Knowledge refinement — *IS_STEP_NUMBER* — 2
- Test Plan definition — *IS_STEP_NUMBER* — 3
- Test Plan implement — *IS_STEP_NUMBER* — 4
- Test execution — *IS_STEP_NUMBER* — 5
- Test Evaluation definition — *IS_STEP_NUMBER* — 6
- Test Evaluation support — *IS_STEP_NUMBER* — 7
- Test Completion Report — *IS_STEP_NUMBER* — 8
- Test triage — *IS_STEP_NUMBER* — 9
- Test triage support — *IS_STEP_NUMBER* — 10
- interrogate-business — *IS_A* — Skill
- interrogate-business — *IS_A* — Polaris AI skills
- interrogate-technical — *IS_A* — Skill
- interrogate-technical — *IS_A* — Polaris AI skills
- graphify-investigate — *IS_A* — Skill
- graphify-investigate — *IS_A* — Polaris AI skills
- interrogate-qa — *IS_A* — Skill
- interrogate-qa — *IS_A* — Polaris AI skills
- review-testability — *IS_A* — Skill
- review-testability — *IS_A* — Polaris AI skills
- story-to-bdd-scenarios — *IS_A* — Skill
- story-to-bdd-scenarios — *IS_A* — Polaris AI skills
- write-acceptance-tests — *IS_A* — Skill
- write-acceptance-tests — *IS_A* — Polaris AI skills
- implement-bdd-steps — *IS_A* — Skill
- implement-bdd-steps — *IS_A* — Polaris AI skills
- luz-docs-integration-test — *IS_A* — Skill
- luz-docs-integration-test — *IS_A* — Polaris AI skills
- playwright-klara-earchive — *IS_A* — Skill
- playwright-klara-earchive — *IS_A* — Polaris AI skills
- google-skill-gke-monitor — *IS_A* — Skill
- google-skill-gke-monitor — *IS_A* — Polaris AI skills
- luz-skill-flow-logs — *IS_A* — Skill
- luz-skill-flow-logs — *IS_A* — Polaris AI skills
- write-test-completion-report — *IS_A* — Skill
- write-test-completion-report — *IS_A* — Polaris AI skills
- qa-html-before-delivery — *IS_A* — Skill
- qa-html-before-delivery — *IS_A* — Polaris AI skills
- grounded-bug-report — *IS_A* — Skill
- grounded-bug-report — *IS_A* — Polaris AI skills
- Knowledge gathering — *USES_SKILL* — interrogate-business
- Knowledge gathering — *USES_SKILL* — interrogate-technical
- Knowledge gathering — *USES_SKILL* — graphify-investigate
- Knowledge gathering — *USES_DATA_STORE* — Polaris Memory Bank
- Knowledge refinement — *USES_SKILL* — interrogate-qa
- Test Plan definition — *USES_SKILL* — interrogate-qa
- Test Plan definition — *USES_SKILL* — review-testability
- Test Plan implement — *USES_SKILL* — story-to-bdd-scenarios
- Test Plan implement — *USES_SKILL* — write-acceptance-tests
- Test execution — *USES_SKILL* — implement-bdd-steps
- Test execution — *USES_SKILL* — luz-docs-integration-test
- Test execution — *USES_SKILL* — playwright-klara-earchive
- Test Evaluation definition — *USES_SKILL* — review-testability
- Test Evaluation support — *USES_SKILL* — google-skill-gke-monitor
- Test Evaluation support — *USES_SKILL* — luz-skill-flow-logs
- Test Completion Report — *USES_SKILL* — write-test-completion-report
- Test Completion Report — *USES_SKILL* — qa-html-before-delivery
- Test triage — *USES_SKILL* — grounded-bug-report
- Test triage support — *USES_SKILL* — grounded-bug-report
- Polaris Memory Bank — *IS_A* — Data store
- Polaris Memory Bank — *INCLUDES* — Agent Memory
- Jira — *IS_A* — Data store
- Xray — *IS_A* — Data store
- Confluence — *IS_A* — Data store
- Agent Memory — *IS_A* — Data store
- Git — *IS_A* — Data store
- GCS — *IS_A* — Data store
- GCS — *STORES* — evidence
- Bitbucket PR — *IS_A* — Trigger
- Bitbucket merge — *IS_A* — Trigger
- nightly-cron — *IS_A* — Trigger
- Bitbucket PR — *TRIGGERS* — Cloud Build
- Bitbucket merge — *TRIGGERS* — Cloud Build
- nightly-cron — *TRIGGERS* — Cloud Build
- LUZ-159671 test-agent — *MAPS_TO* — Knowledge gathering
- LUZ-159671 test-agent — *MAPS_TO_PHASE* — KNOWLEDGE
- GCS-markdown memory bank — *IMPLEMENTS* — Polaris Memory Bank
- vinnstack SKILL.md convention — *IS_RELATED_TO* — beforeafterai
- Knowledge-Gathering loop is a bounded frontier crawl with a verify edge — *IS_RELATED_TO* — beforeafterai

%% ai-graph-end %%