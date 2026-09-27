---
ai_hash: dc5bc17303f1a172
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49318330369'
confluence_path: Team Kepler > Developer note > AI Research > Prompt Engineer
created: 2026-04-12
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- prompt-engineering
- search
title: 'Skill-based Compression Techniques: Overview'
type: source
updated: 2026-04-12
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49318330369/Skill-based+Compression+Techniques+Overview
---

# Skill-based Compression Techniques: Overview

*Confluence source · Team Kepler › Developer note › AI Research › Prompt Engineer · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49318330369/Skill-based+Compression+Techniques+Overview) · updated 2026-04-12*

## Why Skills Are the Best Compression Tool

### The Problem

CLAUDE.md and GEMINI.md are loaded into **every single message**. A 2,000-line config file burns ~8,000 tokens per turn. Over a 20-turn session on Claude Sonnet 4, that is:

```
8,000 tokens x 20 turns x $3.00/MTok = $0.48 wasted per session
At 10 sessions/day = $4.80/day = $144/month -- just on config overhead
```

### The Solution: Skills

Skills load only when invoked. The same 2,000 lines of specialized instructions cost **zero tokens** until you actually need them.

|  |  |  |  |
|----|----|----|----|
| Approach | Tokens Per Turn | 20-Turn Cost | When Loaded |
| Everything in CLAUDE.md/GEMINI.md | ~8,000 | \$0.48 | Every turn |
| Core rules only in config + Skills | ~800 (config) + ~2,000 (skill, 1 turn) | \$0.068 | Config: every turn; Skill: once when invoked |
| **Savings** |  | **86%** |  |

### Both Tools Support Skills

|  |  |  |
|----|----|----|
| Feature | Claude Code | Gemini CLI |
| **Skill standard** | Agent Skills ([http://agentskills.io](http://agentskills.io) ) | Agent Skills (same standard) |
| **Skill file** | `SKILL.md` | `SKILL.md` |
| **Location (personal)** | `~/.claude/skills/<name>/SKILL.md` | `~/.gemini/skills/<name>/SKILL.md` |
| **Location (project)** | `.claude/skills/<name>/SKILL.md` | `.gemini/skills/<name>/SKILL.md` |
| **Invoke** | `/skill-name` or auto-detected | `/skills` menu or auto-detected |
| **Auto-loading** | Yes (by description match) | Yes (model decides from description) |
| **Supporting files** | templates, scripts, examples | scripts, references, assets |
| **Slash command** | Yes (`/name`) | Yes (`/skills` + name) |
| **Subagent mode** | `context: fork` | N/A |

## Claude Code Skills for Compression

### How Claude Code Skills Work

1.  **Discovery:** Skill names + descriptions loaded into context (~250 chars each, dynamically budgeted)

2.  **Invocation:** User types `/skill-name` or Claude auto-loads when relevant

3.  **Loading:** Full SKILL.md content enters conversation as a single message

4.  **Persistence:** Stays for session; carried forward during compaction (first 5,000 tokens, 25K total budget)

5.  **Cost:** Zero until invoked; only description costs ongoing (~50 tokens)

### Skill Directory Structure

```
.claude/skills/
├── review-pr/
│   ├── SKILL.md              # Main instructions (required)
│   ├── checklist.md          # PR review checklist (loaded on demand)
│   └── examples/
│       └── good-review.md    # Example output
├── deploy/
│   ├── SKILL.md
│   └── scripts/
│       └── deploy.sh         # Script Claude can execute
├── compress-context/
│   └── SKILL.md              # Prompt compression skill
└── debug/
    └── SKILL.md
```

### SKILL.md Format (Claude Code)

```
---
name: skill-name
description: What this skill does (under 250 chars, front-load key use case)
disable-model-invocation: false   # true = manual only
user-invocable: true              # false = Claude auto-only
allowed-tools: Read Grep Bash(*)  # Pre-approved tools
context: fork                     # Optional: run in subagent
agent: Explore                    # Optional: agent type for fork
effort: medium                    # Optional: override effort level
model: haiku                      # Optional: use cheaper model
---

Skill instructions in markdown...
```

### Key Frontmatter for Compression

|  |  |
|----|----|
| Field | Compression Impact |
| `effort: low` | Reduces thinking tokens (30-80% savings on reasoning) |
| `model: haiku` | Uses cheapest model for this skill (0.80/4 vs 3/15) |
| `context: fork` | Isolates verbose output in subagent context |
| `disable-model-invocation: true` | Prevents accidental loading (saves context) |

### Compaction Behavior

When context is compacted, Claude Code re-attaches invoked skills:

- First **5,000 tokens** of each skill preserved

- Combined budget: **25,000 tokens** across all active skills

- Most recently invoked skill gets priority

- Older skills may be dropped if budget exceeded

## Gemini CLI Skills for Compression

### How Gemini CLI Skills Work

1.  **Discovery:** Skill names + descriptions injected into system prompt

2.  **Activation:** Model calls `activate_skill` tool when it detects a matching task

3.  **User confirmation:** Prompt shown before loading (can be approved)

4.  **Loading:** SKILL.md body + folder structure added to conversation

5.  **Cost:** Zero until activated; only names/descriptions cost ongoing

### Skill Directory Structure

```
.gemini/skills/
├── code-reviewer/
│   ├── SKILL.md              # Main instructions (required)
│   ├── scripts/              # Executable scripts
│   ├── references/           # Static docs
│   └── assets/               # Templates, data
├── deploy-app/
│   └── SKILL.md
└── compress-session/
    └── SKILL.md
```

### SKILL.md Format (Gemini CLI)

```
---
name: code-reviewer
description: Review code for quality, security, and performance issues. Supports local changes and remote PRs.
---

When reviewing code:

1. Check for security vulnerabilities (OWASP Top 10)
2. Evaluate performance implications
3. Verify test coverage
4. Check for style consistency
...
```

### Skill Management Commands

|  |  |
|----|----|
| Command | Purpose |
| `/skills list` | View all discovered skills |
| `/skills link <path>` | Symlink local skills for development |
| `/skills enable <name>` | Enable a skill |
| `/skills disable <name>` | Disable a skill (removes from discovery) |
| `/skills reload` | Refresh skill discovery |
| `gemini skills install <source>` | Install from Git, local path, or .skill file |
| `gemini skills uninstall <name>` | Remove a skill |

### Creating Skills Automatically

Gemini CLI has a built-in `skill-creator` skill:

```
"Create a new skill called 'code-reviewer'"
```

This auto-generates the directory, SKILL.md with proper frontmatter, and standard resource folders.

## Side-by-Side Comparison

### Feature Comparison

|  |  |  |
|----|----|----|
| Feature | Claude Code Skills | Gemini CLI Skills |
| **Standard** | Agent Skills ([http://agentskills.io](http://agentskills.io) ) | Agent Skills (same) |
| **Entry file** | SKILL.md | SKILL.md |
| **Frontmatter** | name, description, effort, model, context, allowed-tools, hooks, paths, shell | name, description |
| **Personal location** | `~/.claude/skills/` | `~/.gemini/skills/` |
| **Project location** | `.claude/skills/` | `.gemini/skills/` |
| **Slash command** | `/name` (direct invoke) | Via `/skills` menu |
| **Auto-invoke** | By description match | Model calls `activate_skill` |
| **User confirmation** | No (auto-loads) | Yes (approval prompt) |
| **Subagent mode** | `context: fork` + `agent: Explore/Plan` | Not available |
| **Model override** | `model: haiku` per skill | Not available |
| **Effort override** | `effort: low/medium/high` per skill | Not available |
| **Tool pre-approval** | `allowed-tools: Read Grep` | Not available |
| **Dynamic injection** | `` !`command` `` runs shell before loading | Not available |
| **Compaction survival** | First 5K tokens, 25K budget | Survives /compress (as loaded context) |
| **Install from remote** | Via plugins | `gemini skills install <git-url>` |
| **Supporting files** | templates, examples, scripts | scripts, references, assets |
| **Enterprise distribution** | Managed settings | Extension packages |

### Compression Capabilities

|  |  |  |
|----|----|----|
| Compression Feature | Claude Code | Gemini CLI |
| Move workflows out of config | Yes (CLAUDE.md -\> Skills) | Yes (GEMINI.md -\> Skills) |
| Per-skill model selection | Yes (`model: haiku`) | No |
| Per-skill effort control | Yes (`effort: low`) | No |
| Subagent isolation | Yes (`context: fork`) | No |
| Pre-filter via shell | Yes (`` !`command` ``) | No |
| Argument templating | Yes (`$ARGUMENTS`, `$0`, `$1`) | Limited |

### Before/After Token Impact

|  |  |  |  |
|----|----|----|----|
| Metric | Before (all in config) | After (config + Skills) | Savings |
| CLAUDE.md / GEMINI.md size | ~2,000 lines (~8K tokens) | ~50 lines (~200 tokens) | 97.5% per turn |
| Tokens per turn (config) | 8,000 | 200 | 97.5% |
| Tokens when reviewing PR | 8,000 (config already loaded) | 200 (config) + 2,000 (skill) = 2,200 | 72.5% |
| 20-turn session (no skills used) | 160,000 | 4,000 | **97.5%** |
| 20-turn session (2 skills used) | 160,000 | 4,000 + 4,000 = 8,000 | **95%** |

## Practical Skill Examples

### Skill 1: Smart Compaction (Claude Code)

A skill that performs **guided context compression with domain-aware preservation** -- unlike the default `/compact` which applies a generic summary, this skill classifies every piece of context into three tiers (preserve / summarize / discard) based on its value for ongoing engineering work.

When to Use:

- Context usage exceeds 100K tokens (~50% of Sonnet 4's 200K window)

- Before auto-compaction triggers at ~83.5% (avoids losing critical detail)

- After completing a sub-task to shed exploration noise

- When switching focus between features/bugs within the same session

Why It Beats Default /compact

|  |  |  |
|----|----|----|
| Aspect | Default `/compact` | Smart Compaction Skill |
| Summarization rules | Generic "preserve essentials" | Explicit 3-tier taxonomy (preserve/summarize/discard) |
| Code changes | May paraphrase | **Always preserved verbatim with file paths + line numbers** |
| Test results | May summarize | **Key pass/fail + error messages kept intact** |
| Tool output | May keep verbose results | **Outcomes only, raw output discarded** |
| Reporting | No metrics | **Reports tokens before/after + savings %** |
| Invocation | Manual only | Manual only (`disable-model-invocation: true`) |
| Model cost | Uses session model | `effort: low` -- cheaper thinking |

```
# .claude/skills/smart-compact/SKILL.md
---
name: smart-compact
description: Intelligently compact conversation preserving code changes,
test results, and decisions. Use when context exceeds 100K tokens.
effort: low
disable-model-invocation: true
---

Perform an intelligent context compaction:

1. **Preserve** (must keep verbatim):
   - All code changes with file paths and line numbers
   - Test results: which tests pass/fail and key error messages
   - Architectural decisions and their rationale
   - Current task state and next steps
   - Bug root causes and fixes applied

2. **Summarize** (condense to key points):
   - File exploration results (just note which files were read)
   - Debugging attempts (just note what worked and what didn't)
   - Tool output (just note the outcome)

3. **Discard** (safe to remove):
   - Verbose grep/search results already acted on
   - Full file contents already processed
   - Intermediate reasoning that led to a final decision
   - Repetitive confirmation messages

After compaction, report: tokens before, tokens after, savings percentage.
```

### Skill 2: Smart Compression (Gemini CLI)

```
# .gemini/skills/smart-compress/SKILL.md
---
name: smart-compress
description: Intelligently compress conversation preserving
code changes, decisions, and test results.
---

Before compressing, save critical context to memory:

1. Run `/memory add` for each of these (if present):
   - Architectural decisions made in this session
   - Bug root causes identified
   - Code changes and their file paths
   - Current task state and what remains

2. Then run `/compress`

3. After compression, verify key context by running `/memory show`

This ensures no critical information is lost during compression.
```

### Skill 3: Token-Efficient Code Review (Claude Code)

A code review skill optimized for **minimal token consumption on both input and output** -- runs in an isolated subagent context, reads only changed files, and produces a strict tabular output with no prose overhead.

The Problem with Default Code Reviews

When you ask Claude to "review this code", default behavior is expensive:

|                                      |                        |
|--------------------------------------|------------------------|
| Default Behavior                     | Token Cost             |
| Explores related files for context   | +20-50K input tokens   |
| Produces verbose prose analysis      | +2-5K output tokens    |
| Includes preamble ("I'll review...") | +50-100 tokens         |
| Adds caveats and disclaimers         | +100-300 tokens        |
| Provides summary at the end          | +200-500 tokens        |
| Uses full thinking budget            | +5-20K thinking tokens |
| **Total per review**                 | **~30-75K tokens**     |

How Lean Review Solves This

|  |  |  |
|----|----|----|
| Optimization | Technique | Savings |
| Isolate context | `context: fork` + `agent: Explore` -- runs in subagent, doesn't pollute main session | 100% of main context |
| Limit exploration | "Read only changed files (don't explore the whole codebase)" | 60-80% input |
| Strict output format | Markdown table only -- no prose | 70-90% output |
| Issue cap | "Maximum 10 issues per review" | Bounds worst case |
| Eliminate preamble | "No explanations, no caveats, no summary" | 10-20% output |
| Skip noise | "Skip style-only issues unless they cause bugs" | 30-50% signal/noise |
| Low effort | `effort: low` -- less thinking | 30-80% thinking tokens |

```
# .claude/skills/lean-review/SKILL.md
---
name: lean-review
description: Token-efficient code review.
Produces structured, concise output.
effort: low
context: fork
agent: Explore
---

Review the code changes with minimal token usage:

**Output format** (strict -- no prose, no preamble):

| File | Line | Severity | Issue | Fix |
|---|---|---|---|---|
| path | N | HIGH/MED/LOW | description | fix |

**Rules:**
- Maximum 10 issues per review
- One sentence per issue, one sentence per fix
- No explanations, no caveats, no summary
- Skip style-only issues unless they cause bugs
- Read only changed files (don't explore the whole codebase)

Review: $ARGUMENTS
```

```
# .gemini/skills/lean-review/SKILL.md
---
name: lean-review
description: Token-efficient code review producing
structured table output with minimal prose.
---

Review the specified code with minimal token usage.

**Output format** (strict):

| File | Line | Severity | Issue | Fix |
|---|---|---|---|---|
| path | N | HIGH/MED/LOW | one sentence | one sentence |

Rules:
- Maximum 10 issues
- No preamble, no summary, no caveats
- Skip style-only issues
- Read only the files specified
```

### Skill 5: Filtered Test Runner (Claude Code)

A skill that runs tests in an **isolated subagent context** and returns only failure summaries -- preventing verbose test output (which can easily exceed 50K tokens for medium-sized projects) from polluting your main conversation history.

##### The Problem: Test Output Explosion

A single `npm test` run on a medium TypeScript project can produce:

|  |  |
|----|----|
| Output Category | Typical Tokens |
| Passing test names (200 tests) | ~3K |
| Framework boilerplate (Jest/Pytest banners, timings, progress bars) | ~2K |
| Coverage reports | ~5-10K |
| Stack traces for failures (10+ lines each) | ~5K |
| Console.log output from tests | ~5-20K |
| Deprecation warnings | ~1K |
| ANSI color codes + formatting | ~2K |
| **Total raw output** | **~25-50K tokens** |

Every time tests are run without filtering, this entire blob enters your main conversation, **permanently consuming that context budget for the rest of the session**.

How /test-lean Solves This

|  |  |  |
|----|----|----|
| Optimization | Mechanism | Savings |
| Isolate execution | `context: fork` -- test output stays in subagent | 100% of raw output |
| Pre-approve tools | `allowed-tools: Bash(npm test *) Bash(pytest *)` -- no permission prompts | Latency + interruptions |
| Low effort | `effort: low` -- parsing output, not reasoning | 30-80% thinking |
| Filter to failures only | "Do NOT include passing test details" | 60-80% |
| Truncate traces | "first 3 lines only" of stack traces | 70-90% of trace size |
| Strip boilerplate | "Do NOT include test framework boilerplate" | 10-20% |
| Manual invocation | `disable-model-invocation: true` -- Claude can't accidentally run tests | Prevents accidental cost |

|  |  |  |
|----|----|----|
| Field | Value | Purpose |
| `name` | `test-lean` | Becomes `/test-lean` slash command |
| `description` | Auto-discovery hint for test-related requests | Claude suggests it when user wants to run tests |
| `effort` | `low` | Output parsing is pattern matching, not deep reasoning |
| `context` | `fork` | **Critical:** keeps test output out of main session |
| `allowed-tools` | `Bash(npm test *) Bash(pytest *) ...` | Pre-approves test commands -- no permission prompts mid-run |
| `disable-model-invocation` | `true` | Manual only -- tests are side-effecting, don't auto-run |

```
# .claude/skills/test-lean/SKILL.md
---
name: test-lean
description: Run tests and report only failures.
Prevents test output from bloating context.
effort: low
context: fork
allowed-tools: Bash(npm test *) Bash(pytest *) Bash(mvn test *) Bash(go test *)
disable-model-invocation: true
---

Run the test suite and report results efficiently:

1. Run: $ARGUMENTS (or detect test command from package.json / pom.xml / go.mod)
2. Filter output to failures only
3. Report in this format:

**Test Results: X passed, Y failed**

| Test | Status | Error |
|---|---|---|
| test name | FAIL | one-line error description |

- Do NOT include passing test details
- Do NOT include full stack traces (first 3 lines only)
- Do NOT include test framework boilerplate
```

### Skill 6: Architecture Overview (Both CLIs)

A skill that **generates an architecture overview on demand** instead of keeping a 500-line architecture description permanently loaded. The overview is generated fresh each time it's invoked, reflecting the **current** state of the codebase -- no stale docs.

Many teams put their architecture docs directly in their always-loaded config file. This has three compounding problems:

|  |  |
|----|----|
| Problem | Impact |
| **Always loaded** | 500-line architecture doc = ~2,000 tokens per turn, every turn |
| **Goes stale** | Codebase evolves; docs don't. Claude reads stale info and makes incorrect assumptions |
| **Overkill** | Most turns don't need architecture context at all (bug fixes, small edits, etc.) |

##### How /architecture Solves This

|  |  |  |
|----|----|----|
| Optimization | Technique | Savings |
| Remove from config | Architecture docs move out of CLAUDE.md/GEMINI.md | ~2K tokens/turn eliminated |
| On-demand generation | Fresh scan reflects current code | Always accurate |
| Isolated execution | `context: fork` + `agent: Explore` | Scanning doesn't pollute main |
| Compact output | Strict table format, no prose | 70-90% less than verbose description |
| Low effort | `effort: low` -- file scanning, not reasoning | 30-80% thinking tokens |

|  |  |  |
|----|----|----|
| Field | Value | Purpose |
| `name` | `architecture` | Becomes `/architecture` slash command |
| `description` | Auto-discovery hint for exploration/planning | Claude suggests it when user joins new area |
| `effort` | `low` | File scanning is pattern matching, not reasoning |
| `context` | `fork` | Keeps the 20-50 file reads out of main session |
| `agent` | `Explore` | Read-only tools optimized for codebase discovery |

**Claude Code:**

```
# .claude/skills/architecture/SKILL.md
---
name: architecture
description: Show project architecture overview.
Use when exploring a new area or planning changes.
effort: low
context: fork
agent: Explore
---

Generate a concise architecture overview:

1. Read the project root files (package.json, pom.xml, go.mod, etc.)
2. List top-level directories with one-line purpose descriptions
3. Identify key entry points and configuration files
4. Note the tech stack (framework, database, testing)

Output format:
## Architecture
**Stack:** [framework, language, DB, test]
**Entry:** [main entry point]

| Directory | Purpose | Key Files |
|---|---|---|
| src/api/ | REST endpoints | routes.ts, middleware.ts |
| ... | ... | ... |
```

**Gemini CLI:**

```
# .gemini/skills/architecture/SKILL.md
---
name: architecture
description: Generate project architecture overview. Use when exploring a new area or planning changes.
---

Generate a concise architecture overview:

1. Scan the project root for build files and config
2. List directories with one-line purpose descriptions
3. Identify tech stack and entry points

Output as a compact table:
| Directory | Purpose | Key Files |
```

### Skill 7: Prompt Optimizer (Claude Code)

#### Skill 7: Prompt Optimizer (Claude Code)

A **meta-skill** that compresses prompts before they're sent to any LLM. Instead of manually applying 8 different text compression techniques from §04 and §06 of this docs folder, you delegate the work to Claude itself -- paste a verbose prompt, get back a compressed version with token counts and savings percentage.

This skill performs a task on your **prompts** -- it's an AI that optimizes how you talk to AI. That inversion makes it uniquely valuable for:

- **Prompt template library maintenance** -- systematically compress every template you use

- **Production prompt tuning** -- reduce cost on prompts that run millions of times

- **Learning** -- see why your prompts are verbose by comparing before/after

- **Batch optimization** -- compress a whole library of existing prompts

Most developers don't know how verbose their prompts are. A typical "quick question" to Claude might cost 2x what it needs to:

|  |  |  |
|----|----|----|
| Prompt Type | Typical Waste | Common Culprits |
| Ad-hoc debugging prompts | 30-50% | Filler phrases, unnecessary context |
| Production system prompts | 40-60% | Redundant instructions, verbose examples |
| Few-shot example sets | 50-70% | Prose reasoning in examples, verbose inputs |
| Agent orchestration prompts | 30-50% | Hedging, meta-talk, "if that makes sense" |
| RAG retrieval prompts | 20-40% | Boilerplate context wrappers |

|  |  |  |
|----|----|----|
| Optimization | Technique | Savings |
| All techniques in one pass | Claude applies filler removal, structure, abbreviations, etc. simultaneously | 30-80% per prompt |
| Quantified results | Before/after token counts + savings % | Measurable impact |
| Low effort | `effort: low` -- pattern matching, not reasoning | 30-80% thinking tokens |
| Manual invocation | `disable-model-invocation: true` | Prevents accidental compression of user messages |
| Variable input | `$ARGUMENTS` -- works on any prompt | Reusable across all templates |

|  |  |  |
|----|----|----|
| Field | Value | Purpose |
| `name` | `optimize-prompt` | Becomes `/optimize-prompt` slash command |
| `description` | Advertises token-efficiency optimization | Helps Claude auto-suggest when user pastes verbose prompt |
| `effort` | `low` | Applying 8 rules is pattern matching -- no deep reasoning needed |
| `disable-model-invocation` | `true` | **Critical:** prevents Claude from auto-compressing user messages (which would be confusing) |

```
# .claude/skills/optimize-prompt/SKILL.md
---
name: optimize-prompt
description: Optimize a prompt for token efficiency.
Reduces filler, restructures, compresses.
effort: low
disable-model-invocation: true
---

Optimize the following prompt for token efficiency:

$ARGUMENTS

Apply these transformations:
1. Remove all filler phrases ("I would appreciate", "Could you please", etc.)
2. Replace prose instructions with structured format (bullets, tables)
3. Replace verbose examples with schema descriptions
4. Use abbreviations for repeated terms
5. Remove redundant instructions

Output:
- **Original tokens:** (estimate)
- **Optimized tokens:** (estimate)
- **Savings:** X%
- **Optimized prompt:**
[the compressed prompt]
```

## Migration Guide

### What to Move from CLAUDE.md to Skills

|                                       |                   |                    |
|---------------------------------------|-------------------|--------------------|
| Content Type                          | Keep in CLAUDE.md | Move to Skill      |
| Project name + tech stack (3 lines)   | Yes               |                    |
| Core coding rules (10 lines)          | Yes               |                    |
| Compact instructions (5 lines)        | Yes               |                    |
| PR review checklist (50 lines)        |                   | `/review-pr`       |
| Deployment procedure (100 lines)      |                   | `/deploy`          |
| Database migration guide (80 lines)   |                   | `/migrate-db`      |
| API documentation (200 lines)         |                   | `/api-docs`        |
| Testing strategy (60 lines)           |                   | `/test-guide`      |
| Debugging playbook (40 lines)         |                   | `/debug` (bundled) |
| Code generation templates (100 lines) |                   | `/generate`        |

### What to Move from GEMINI.md to Skills

|                                        |                   |                 |
|----------------------------------------|-------------------|-----------------|
| Content Type                           | Keep in GEMINI.md | Move to Skill   |
| Tech stack + global rules (5 lines)    | Yes               |                 |
| Project overview (5 lines)             | Yes               |                 |
| PR review workflow (50 lines)          |                   | `code-reviewer` |
| Deployment steps (80 lines)            |                   | `deploy-app`    |
| Architecture documentation (200 lines) |                   | `architecture`  |
| Onboarding guide (100 lines)           |                   | `onboarding`    |

### Migration Steps (Claude Code)

```
# 1. Create skill directory
mkdir -p .claude/skills/review-pr

# 2. Move content from CLAUDE.md to SKILL.md
# Cut the PR review section from CLAUDE.md and paste into:
cat > .claude/skills/review-pr/SKILL.md << 'EOF'
---
name: review-pr
description: Review a PR with security, performance, and style checks.
disable-model-invocation: true
---

[paste your PR review checklist here]
EOF

# 3. Trim CLAUDE.md to essentials only
# Target: under 200 lines / ~800 tokens
```

### Migration Steps (Gemini CLI)

```
# 1. Create skill directory
mkdir -p .gemini/skills/code-reviewer

# 2. Create SKILL.md
cat > .gemini/skills/code-reviewer/SKILL.md << 'EOF'
---
name: code-reviewer
description: Review code for quality, security, and performance issues.
---

[paste your code review workflow here]
EOF

# 3. Trim GEMINI.md to essentials only

# 4. Reload skills
# In Gemini CLI: /skills reload
```

## Advanced Patterns

### Pattern 1: Tiered Effort Skills (Claude Code)

Create skills at different cost tiers:

```
# Quick check: low effort, cheap model
---
name: quick-check
effort: low
model: haiku
context: fork
---

# Deep analysis: high effort, strong model
---
name: deep-analysis
effort: high
---
```

### Pattern 2: Dynamic Context Injection (Claude Code)

Pre-compute expensive context before the skill loads:

```
---
name: pr-summary
context: fork
agent: Explore
---

## Live PR Data
- Diff: !`gh pr diff`
- Files changed: !`gh pr diff --name-only`
- Comments: !`gh pr view --comments`

Summarize this PR concisely.
```

The shell commands run **before** Claude sees the prompt. Claude gets pre-computed data, not raw commands.

### Pattern 3: Skill Chaining

Invoke one skill from another:

```
/lean-review src/auth/   -> Produces issue table
/fix-issue 1             -> Fixes issue #1 from the review
/test-lean npm test      -> Runs tests in isolated context
```

Each skill loads only when invoked, keeping context lean between steps.

### Pattern 4: Compression Skill Pipeline (Claude Code)

```
---
name: compress-pipeline
description: Full compression pipeline: compact, filter, summarize.
disable-model-invocation: true
effort: low
---

Execute this compression pipeline:
1. Check current context size via /context
2. If > 100K tokens:
   a. Save key decisions to CLAUDE.md (temporary section)
   b. Run /compact with focus on: code changes, test results, decisions
   c. Remove the temporary CLAUDE.md section
3. Report: before tokens, after tokens, savings %
```

%% ai-graph-start %%

**Related notes:**
- [[Skill-based Compression Techniques - Overview]]
- [[Claude Code Skill anatomy]]
- [[Manual Prompt Compression Techniques - Deep Dive]]
- [[Manual Prompt Compression Techniques - Deep Dive]]
- [[Knowledge Base Solutions Comparison Guide]]

%% ai-graph-end %%