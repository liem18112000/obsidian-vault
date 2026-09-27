---
ai_hash: 24f5527857e66af1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49754406963'
confluence_path: 'Team Kepler > AI-First Framework — Mission Team: Receive > Testing
  Agents > Agent Loop 1 - Knowledge Gathering - v2 > Evaluating Knowledge-Gathering
  Agent - V2'
created: 2026-09-15
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
title: Judge Calibration and Canary Seeds
type: source
updated: 2026-09-15
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49754406963/Judge+Calibration+and+Canary+Seeds
---

# Judge Calibration and Canary Seeds

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › Agent Loop 1 - Knowledge Gathering - v2 › Evaluating Knowledge-Gathering Agent - V2 · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49754406963/Judge+Calibration+and+Canary+Seeds) · updated 2026-09-15*

## **Purpose.**

- Both eval reports — the Pack Quality Score and the Test-Plan Score — mix **deterministic** metrics (set overlap, regex, partition presence) with a few **LLM-judged** ones (Faithfulness, `hallucinations_v1`, the semantic rubrics, oracle-depth).

- A judged number is worthless until you can show the judge agrees with a human about as well as two humans agree with each other.

- This page is the shared **calibration protocol** for that judged tier, and the **canary** guard that keeps it honest between recalibrations. It's a sibling concern to both reports, factored out so it lives in one place.

### Sources

- Cohen, J. (1960), "A Coefficient of Agreement for Nominal Scales," *Educational and Psychological Measurement*.

- The three-layer framework, weighted rubric, and calibration protocol: the local `agentic-qa-eval-framework.html` evaluation spec.

- Provider-sourced judge + canary implementation: `src/test_evaluation/eval/judge.py`, `src/test_evaluation/metrics/ragas_judge.py`, `src/test_evaluation/golden.py`, `tests/eval/test_canary.py`.

## Why calibrate at all

- An LLM-as-judge produces a number that *looks* objective. It isn't, until measured.

  - A judge that quietly disagrees with your QA experts half the time will flag good packs and pass bad ones, and its "faithfulness = 0.82" carries no more information than a coin flip dressed as a decimal.

  - Calibration answers one question before you trust any judged score: **does the judge agree with a human about as reliably as two humans agree with each other?**

  - If not, that dimension must stay deterministic (or off).

- This is why in this system **most of the score is deterministic** — `ctx_precision` / `ctx_recall`,

  - the hard-negative leak gate, entity recall, coverage, oracle strength, placeholder validity — and only a **few surfaces are judged**:

    - Faithfulness / Response Relevancy (RAGAS),

    - `hallucinations_v1` (ADK),

    - the semantic rubrics `names_the_ac` / `declares_gaps_honestly`,

    - the LLM oracle-depth classifier.

  - The deterministic metrics need no calibration; the judged ones do.

## What Cohen's kappa is (and how to read it)

Cohen's κ measures how much two raters agree on a **categorical** label (relevant / irrelevant; grounded / not), **corrected for chance**. Plain percent-agreement lies when one label dominates: if 90% of understandings are "grounded", two raters who both always say "grounded" agree 90% of the time while adding zero signal. Kappa subtracts that chance floor:

```
κ = (p_o − p_e) / (1 − p_e)
      p_o = observed agreement   (how often the two raters gave the same label)
      p_e = agreement by chance  (from each rater's base rates)
```

Read it as:

|               |                                    |
|---------------|------------------------------------|
| κ             | meaning                            |
| **1.0**       | perfect agreement                  |
| **0.8 – 1.0** | near-perfect                       |
| **0.6 – 0.8** | substantial                        |
| **0.4 – 0.6** | moderate                           |
| **0.2 – 0.4** | fair                               |
| **≤ 0**       | no better than / worse than chance |

![[image-20260915-042047.png]]

## The protocol

1.  **Two humans score ~30 artifacts**

    - on the same rubric, independently — per-node relevant/irrelevant for the pack, per-sentence grounded/not for the understanding or brief.

2.  **Compute human–human κ. This is the ceiling.**

    - It bounds how well *anything* can score that dimension.

    - If two experts only reach κ ≈ 0.4, the *task* is ambiguous — they read the rubric differently — and no judge can be reliably "more right" than the humans are consistent.

    - **A low human–human κ is a rubric-wording defect, not a judge defect: rewrite the rubric until experts converge.**

    - Automating an ambiguous rubric just launders disagreement into a number that looks objective but isn't reproducible.

3.  **Measure judge–human κ the same way, and gate:**

    - `judge–human κ ≥ human–human κ − ~0.1`**.** The judge needn't beat humans — only be about as consistent with a human as humans are with each other, minus a small slack.

    - **Clears the bar** → trustworthy enough to run that dimension **judged, nightly**.

    - **Falls below** → the judge adds noise for that dimension → keep it **deterministic** (set-overlap / regex) or turn it **off**.

4.  **Pin the judge model and re-calibrate on change.**

    - A κ measured on one model version doesn't transfer.

    - Pin the exact model; re-measure κ on any model or prompt change.

![[image-20260915-045953.png]]

## How it maps to this system

- **The judge is provider-sourced (I8).**

  - Every judged path sources its model from the one configured `ModelProvider` — Claude-on-Vertex via `agent_model()`.

  - `ragas_judge.judge` *raises* on a `None` llm rather than silently falling back;

  - `eval/config.judged_criteria()` returns `{}` (a clean skip) when no provider is configured.

  - So a judged run is either the pinned model or nothing.

- **The pinned version is inspectable.**

  - `test_evaluation.eval.judge.judge_model_id()` returns exactly the provider model id used as `JudgeModelOptions.judge_model` — that is what a κ measurement is pinned to.

- **Deterministic vs judged is already split.**

  - The deterministic tier (which needs no calibration) is the PR gate and carries most of the PQS/TPS weight;

  - the judged tier runs nightly / on the `eval:` label and skips without the `[eval]` extra or `VERTEX_*` creds.

  - Calibration decides only which side of that line a dimension sits on.

- **The judged rubrics are declared as data.**

  - `metrics/rubrics.SEMANTIC_RUBRICS` (`names_the_ac`, `declares_gaps_honestly`) are the exact text a human and the judge score against — so "fix the rubric wording" is a one-line edit, and the same text feeds ADK's `rubric_based_final_response_quality_v1`.

## Canary seeds — the continuous drift guard

Full recalibration (30 human-scored artifacts) is expensive; you don't run it every night.

**Canary seeds** are the cheap guard in between: a small set of *deliberately-bad* artifacts (a bled pack, a plan that scopes the excluded nodes) whose score **must stay low**.

Score them every run — if a canary ever scores *high* (or its hard-negative gate stops firing), the scorer is broken, not the agent, and it's time to re-calibrate.

As built:

- Canary cases live under `golden/canary/` and `golden_plans/canary/` — a **subdirectory**, so the top-level `*.json` glob (and therefore `load_golden()` and the ADK evalset) never picks them up. Dedicated loaders `golden.load_canaries()` / `load_canary_plans()` read them.

- Each canary is an ordinary `EvalCase` / `PlanEvalCase` with `canary: true` and a `max_score` ceiling. The shipped `canary_bleed` cases invert the ground truth (declare a node the artifact contains as a hard-negative, the "relevant"/"in-scope" node absent), so a correct metric must score them low **and** fire the leak gate.

- `tests/eval/test_canary.py` runs them through the real engine and asserts `pqs`/`tps ≤ max_score` **and** that `retrieval.leaked` / `scope.leaked` is non-empty. A metric regression that inflates precision or drops the gate turns the canary red.

Canaries validate the *metric*; the calibration protocol validates the *judge*. Together they keep a judged score meaning what it says.

%% ai-graph-start %%

**Related notes:**
- [[Evaluating Knowledge-Gathering Agent - V2]]
- [[Evaluating the Test-Plan-Definition Agent]]
- [[Agent Loop 4 - Test-Plan Execution]]
- [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]]
- [[Test oracle - what a scenario asserts, and why its strength decides everything]]

%% ai-graph-end %%