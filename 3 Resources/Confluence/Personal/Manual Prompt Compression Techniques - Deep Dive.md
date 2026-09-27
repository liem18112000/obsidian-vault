---
title: "Manual Prompt Compression Techniques: Deep Dive"
created: 2026-04-13
updated: 2026-04-13
type: source
status: reference
source: "Confluence · ~71202087b0f7f1aaab4406a25dfa0fc075c4d4 - liem.doanvanthanh"
url: https://axonivy.atlassian.net/wiki/spaces/~71202087b0f7f1aaab4406a25dfa0fc075c4d4/pages/49318461442/Manual+Prompt+Compression+Techniques+Deep+Dive
confluence_id: "49318461442"
confluence_path: "Overview"
tags: [confluence, prompt-engineering]
---

# Manual Prompt Compression Techniques: Deep Dive

*Confluence source · Overview · [view original](https://axonivy.atlassian.net/wiki/spaces/~71202087b0f7f1aaab4406a25dfa0fc075c4d4/pages/49318461442/Manual+Prompt+Compression+Techniques+Deep+Dive) · updated 2026-04-13*

## Core Principles

#### Why Manual Engineering Delivers the Highest ROI

- **Zero cost, infrastructure** better prompts cost nothing to implement

- **Often improves quality** less noise = better model attention allocation

- **Immediate impact** apply right now, see savings on next message

- **Stacks with everything** prompt caching, routing, compression all benefit from leaner input

- **15-60% token reduction** typical, up to 84% in extreme cases

- **Most teams waste 40-60%** of token budgets on suboptimal prompts

|  |  |  |
|----|----|----|
| **Lever** | **Target** | **Typical Savings** |
| Trim input text | Remove filler, redundancy | 15-40% input |
| Structure data efficiently | XML tags, schemas, YAML | 15-30% input |
| Serialize data compactly | TOON, compact JSON, abbreviations | 20-40% input |
| Control output length | max_tokens, stop sequences, format | 30-70% output |
| Load context selectively | Only needed files/data | 50-95% input |

## Input Compression: Text Techniques

### Remove Filler Phrases (5-15% reduction)

|  |  |  |
|----|----|----|
| **Filler** | **Tokens Saved** | **Replace With** |
| "I would really appreciate it if you could please" | 10 | (nothing -- just state the task) |
| "Could you take a look at" | 6 | (nothing) |
| "I'd like you to" | 5 | (nothing) |
| "Please make sure to" | 4 | (nothing) |
| "It would be great if you could" | 7 | (nothing) |
| "In order to" | 3 | "To" |
| "Due to the fact that" | 5 | "Because" |
| "At this point in time" | 5 | "Now" |
| "In the event that" | 4 | "If" |

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **Technique** | **Before** | **Tokens** | **After** | **Tokens** | **Savings** |
| **Remove filler** | "I would really appreciate it if you could please take a look at the following text and provide me with a comprehensive summary that captures all of the main points and key ideas. The summary should be concise but thorough, covering the most important aspects of the text. Here is the text I'd like you to summarize:" | 87 | "Summarize the following text, capturing all main points concisely:" | 14 | **84%** |
| **Consolidate redundant** | "Make sure the code is well-documented. Add comments to explain complex logic. Include docstrings for all functions. Document any non-obvious behavior. Add inline comments for tricky parts." | 42 | "Add docstrings to all functions and inline comments for complex logic." | 13 | **69%** |
| **Prose to constraints** | "The output should be formatted as a JSON object. Each entry should have a 'name' field that is a string, an 'age' field that is a number, and a 'email' field that is a valid email address. Make sure to include all three fields for every entry." | 54 | `Output JSON: {name: string, age: number, email: string}` | 10 | **81%** |
| **Abbreviations** | "Check if UserAuthenticationService correctly validates JWT tokens before querying the PostgreSQL database" | ~18 | "Check if SVC correctly validates JWT tokens before querying DB" (define `SVC`/`DB` once) | ~11 | **39%** |
| **Remove irrelevant context** | "I'm building a web application using React and Node.js. The application is designed for managing customer relationships. We've been working on it for about 6 months now. The team consists of 5 developers. We use Git for version control and JIRA for project management. Anyway, I need help with a specific issue in the login form component..." | ~65 | "In my React app, fix the login form validation in `src/components/Login.tsx`: \[specific issue\]" | ~20 | **~70%** |

## Input Compression: Structural Optimization

### XML Tags for Claude (5-10% + quality improvement)

- Claude is specifically trained to recognize XML tags.

- Using them reduces ambiguity, which means fewer follow-up tokens and better first-attempt quality.

**Why XML helps token efficiency:**

- Reduces hallucinations by up to 40% (fewer retry tokens)

- Better first-attempt quality = fewer follow-up corrections

- Queries at the end improve quality by up to 30% for multi-document inputs

```
<task>Review this code for security vulnerabilities</task>
<context>Spring Boot REST API handling payment data</context>
<constraints>
- Focus on OWASP Top 10
- Severity: HIGH/MEDIUM/LOW
- Include fix for each
</constraints>
`
@PostMapping("/pay")
public ResponseEntity<String> processPayment(@RequestBody PaymentRequest req) {
    String query = "SELECT * FROM payments WHERE id = '" + req.getId() + "'";
    return ResponseEntity.ok(db.execute(query));
}
`
```

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| Technique | Before | Tokens | After | Tokens | Savings |
| **Prose to structured list** | "I need you to create a function that takes a list of numbers as input, filters out any negative numbers, then squares each remaining number, and finally returns the sum of all the squared values." | 43 | `Create function:` `- Input: list[int]` `- Filter: remove negatives` `- Transform: square each` `- Return: sum of squares` | 18 | **58%** |
| **Examples to schema** | "Format the output like this: - Name: John Smith, Age: 30, Role: Engineer, Department: Backend - Name: Jane Doe, Age: 25, Role: Designer, Department: Frontend" | 48 | `Output YAML list of {Name, Age, Role, Department}` | 12 | **75%** |
| **Prose to table** | "React is good for building interactive UIs and has a large community. Vue is simpler and easier to learn but has a smaller community. Angular is more opinionated and better for enterprise but has a steeper learning curve." | 62 | Markdown table: Framework / Strength / Weakness -- 3 rows | 30 | **52%** |

## Input Compression: Data Serialization

Poor serialization wastes 40-70% of tokens on formatting overhead.

### JSON Key Optimization

|  |  |  |
|----|----|----|
| Verbose Key | Compact Key | Tokens Saved |
| `customer_full_name` | `name` | ~3 |
| `customer_email_address` | `email` | ~3 |
| `customer_phone_number` | `phone` | ~3 |
| `customer_account_type` | `type` | ~3 |
| `customer_registration_date` | `reg` | ~3 |
| **Total (5 records)** | Verbose: 286 tokens -\> Compact: 180 tokens | **37% saved** |

### Data Format Comparison (Single Record)

|  |  |  |  |
|----|----|----|----|
| **Format** | **Example (same data)** | **Tokens** | **Savings vs JSON** |
| **JSON** | `{"name": "Alice", "age": 30, "role": "engineer", "dept": "backend"}` | 32 | Baseline |
| **YAML** | `name: Alice` / `age: 30` / `role: engineer` / `dept: backend` | 20 | 38% |
| **TOON** | `Alice|30|engineer|backend` (schema defined once: `name|age|role|dept`) | ~19 | 40% |
| **CSV** | `Alice,30,engineer,backend` (header defined once: `name,age,role,dept`) | ~16 | 50% |

### Data Format Comparison at Scale (5 Records) and Nested Data

|  |  |  |  |
|----|----|----|----|
| **Scenario** | **Format** | **Tokens** | **Savings** |
| **5-row flat data** | JSON array `[{"name":"Alice","score":95}, ...]` | 120+ | Baseline |
| **5-row flat data** | CSV `name,score` + `Alice,95` + ... | 40 | **67%** |
| **5-row flat data** | TOON schema once + `Alice|95` + ... | ~35 | **71%** |
| **Nested config** | JSON `{"server":{"host":"localhost","port":8080,"ssl":true,"workers":4}}` | 45 | Baseline |
| **Nested config** | YAML `server:` / `host: localhost` / `port: 8080` / ... | 28 | **38%** |

TOON (Token-Oriented Object Notation) defines the schema once and sends data compactly. Effective for batch processing.

## Output Control: Reducing Response Tokens

Since output tokens are 4-8x more expensive than input, controlling response length is the highest-leverage cost optimization.

### Set max_tokens

```
# Claude API: Cap output length
response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=500,  # Hard cap on output tokens
    messages=[{"role": "user", "content": "Summarize this article."}],
)
```

```
# Gemini API: Cap output length
response = client.models.generate_content(
    model="gemini-2.5-flash",
    contents="Summarize this article.",
    config={"max_output_tokens": 500},
)
```

### Use Stop Sequences

```
# Stop generating when hitting a delimiter
response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=2000,
    stop_sequences=["---END---", "\n\n\n"],
    messages=[{"role": "user", "content": "List the top 3 issues. End with ---END---"}],
)
```

### Output Prompt Style & Budget Comparison

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **Response Type** | **Verbose Prompt / Output** | **Typical Tokens** | **Optimized Prompt / Output** | **Optimized Tokens** | **Savings** |
| General Q&A | "Tell me about Python" -\> essay | 500+ | "List 3 key features of Python, one line each" -\> list | ~30 | **94%** |
| Classification | "Analyze this code for safety" -\> analysis | 50-200 | "Is this code safe? Answer YES or NO only." -\> enum | 1-10 | **95-99%** |
| Code review | Open prose review | 800-2000 | Structured: `VERDICT: [PASS/FAIL] REASON: [1 sentence] FIX: [1 sentence]` | 200-400 | **75%** |
| Summary | Verbose summary with preamble | 500-1000 | Constrained: "No preamble, no caveats, no closing summary" | 100-200 | **80%** |
| Code generation | Code output | 200-1000 | (can't reduce much -- code is code) | 200-1000 | 0% |
| Q&A (open-ended) | Open-ended answer | 200-800 | Constrained: "Answer in under 3 sentences" | 50-150 | **75%** |

### Output Control Techniques

|  |  |  |
|----|----|----|
| **Technique** | **Example** | **Effect** |
| **max_tokens** | `max_tokens=500` (Claude) / `max_output_tokens=500` (Gemini) | Hard cap on output length |
| **Stop sequences** | `stop_sequences=["---END---", "\n\n\n"]` | Stop generating at delimiter |
| **Structured output** | JSON schema via `tool_use` / `tool_choice` | Forces parseable output, no prose overhead |
| **Exclusion list** | "Do NOT include: introductory phrases, caveats, disclaimers, closing summary" | Eliminates boilerplate |
| **Format template** | "Respond in this exact format: VERDICT: / REASON: / FIX:" | Forces concise, parseable output |

## Few-Shot Optimization

### The Over-Prompting Problem

Token costs increase linearly with each example while accuracy gains flatten. Research shows:

- **Zero-shot often works** for strong models (Claude Sonnet 4, Gemini 2.5 Pro)

- **1-2 examples** match or exceed **5+ examples** in many tasks

- **Example quality \> example quantity**

#### Decision Tree

```
Can zero-shot handle it?
  YES -> Use zero-shot (cheapest)
  NO  -> Add 1-2 high-quality examples
           Still not working?
             -> Add up to 3-5 diverse examples
                Still not working?
                  -> Use structured output / tool_use instead
```

### Compress Examples

|  |  |  |
|----|----|----|
|  | Example Content | Tokens |
| **Before (verbose)** | `<example>` Input: "The product arrived damaged and the customer service representative was unhelpful when I tried to get a replacement. I've been waiting for two weeks with no resolution." Classification: Negative. Reasoning: The customer expresses frustration about both the product quality and the service response time. `</example>` | 80 |
| **After (compressed)** | `<example>` In: "Product damaged, CS unhelpful, 2-week wait, no resolution." Out: Negative `</example>` | 25 |
| **Savings** | Removed verbose input text + reasoning (model infers pattern from compact example) | **69%** |

## Chain-of-Thought Token Efficiency

### The Problem

Chain-of-thought reasoning generates thinking tokens billed at output rates. Without constraints, models may:

- Repeat themselves

- Hedge and wander

- Explore dead ends

- Generate 10x more tokens than the actual answer

### Strategies to Reduce CoT Cost

#### Use Effort Levels (Claude 4.6)

```
# Low effort: minimal thinking, fastest, cheapest
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=4096,
    thinking={"type": "adaptive"},
    output_config={"effort": "low"},      # Simple tasks
    messages=[...],
)
# Medium effort: balanced (recommended default)
output_config={"effort": "medium"},
# High effort: deep reasoning (complex problems only)
output_config={"effort": "high"},
```

#### Zero-Shot CoT Over Few-Shot CoT

Recent research (2025) shows that for strong models like Claude 4.x and Gemini 2.5, zero-shot CoT ("Think step by step") often matches few-shot CoT performance -- saving all the example tokens.

|  |  |  |  |
|----|----|----|----|
| CoT Style | Prompt | Input Tokens | Quality |
| **Few-shot CoT** | "Example 1: To find 20% of 50, I multiply... \[100 tokens\] Example 2: To find 30% of 80... \[100 tokens\] Now: What is 15% of 340?" | ~220 | Baseline |
| **Zero-shot CoT** | "Think step by step. What is 15% of 340?" | ~12 | Same (on strong models) |
| **Savings** |  | **95%** on input |  |

#### Focused & Hierarchical CoT

|  |  |  |
|----|----|----|
| Technique | Prompt | Effect |
| **Focused CoT** | "Solve this in under 3 reasoning steps. Show only the key calculation." | Forces compression into essential steps |
| **Hierarchical** | "First state your approach in one sentence. Then solve. Then state the answer." | Each step forces compression, filters noise |
| **Budget-constrained** | "Explain in under 50 words." | Hard cap on reasoning verbosity |

## Context Loading Strategies

### Context Loading: Before/After Examples

<table>
<tbody>
<tr>
<th><p>**Strategy**</p></th>
<th><p>**Approach**</p></th>
<th><p>**Tokens**</p></th>
<th><p>**Savings**</p></th>
</tr>
&#10;<tr>
<td rowspan="3"><p>**Selective loading**</p></td>
<td><p>Broad: "Read the entire `src/` directory"</p></td>
<td><p>200K+</p></td>
<td><p>Baseline</p></td>
</tr>
<tr>
<td><p>Targeted: "Read `src/index.ts` and `src/routes/`"</p></td>
<td><p>10K</p></td>
<td><p>**95%**</p></td>
</tr>
<tr>
<td><p>Pinpoint: "Read `processOrder` in `src/api/OrderController.java`"</p></td>
<td><p>500</p></td>
<td><p>**99.75%**</p></td>
</tr>
<tr>
<td rowspan="4"><p>**Incremental loading**</p></td>
<td><p>Upfront: "Read `src/auth/*`" (all at once)</p></td>
<td><p>15K</p></td>
<td><p>Baseline</p></td>
</tr>
<tr>
<td><p>Turn 1: "Read `src/auth/login.ts`"</p></td>
<td><p>2K</p></td>
<td></td>
</tr>
<tr>
<td><p>Turn 2: "Now read the session handler it imports"</p></td>
<td><p>+1.5K (3.5K total)</p></td>
<td></td>
</tr>
<tr>
<td><p>Turn 3: "Check the combination for vulnerabilities"</p></td>
<td><p>+0 (3.5K total)</p></td>
<td><p>**77%**</p></td>
</tr>
<tr>
<td rowspan="4"><p>**Pre-filter inputs**</p></td>
<td><p>Raw log file: `app.log`</p></td>
<td><p>200K</p></td>
<td><p>Baseline</p></td>
</tr>
<tr>
<td><p>Filtered: `grep -E "(ERROR|FATAL)" app.log | tail -100`</p></td>
<td><p>500</p></td>
<td><p>**99.75%**</p></td>
</tr>
<tr>
<td><p>Raw test output: full `pytest` run</p></td>
<td><p>50K</p></td>
<td><p>Baseline</p></td>
</tr>
<tr>
<td><p>Filtered: `pytest 2>&1 | grep -A 3 "FAILED"`</p></td>
<td><p>500</p></td>
<td><p>**99%**</p></td>
</tr>
<tr>
<td rowspan="2"><p>**Architecture summary**</p></td>
<td><p>Read all source files to understand codebase</p></td>
<td><p>100K+</p></td>
<td><p>Baseline</p></td>
</tr>
<tr>
<td><p>Load `ARCHITECTURE.md` index file (~500 tokens), then dive into specific dirs</p></td>
<td><p>500 + as needed</p></td>
<td><p>**95%+**</p></td>
</tr>
<tr>
<td rowspan="2"><p>**Batch questions**</p></td>
<td><p>3 separate turns (50K context each): "What does A do?", "What does B do?", "How do A and B interact?"</p></td>
<td><p>150K total</p></td>
<td><p>Baseline</p></td>
</tr>
<tr>
<td><p>1 batched turn: "Explain functions A and B in `src/service.ts` and how they interact"</p></td>
<td><p>50K total</p></td>
<td><p>**67%**</p></td>
</tr>
</tbody>
</table>

## System Prompt Optimization

### Why System Prompt Size Matters

System prompts are sent with **every message**. In a 20-turn conversation:

- 1,000-token system prompt = 20,000 tokens total

- 200-token system prompt = 4,000 tokens total

- **Savings: 16,000 tokens (80%)**

With prompt caching, cached system prompts cost only 10% -- but the cache write on first message still costs 1.25x, and bloated prompts waste cache storage.

### The Lean System Prompt Pattern

|  |  |  |  |
|----|----|----|----|
|  | **System Prompt** | **Tokens** | **20-Turn Cost** |
| **Good (lean)** | "You are a senior Java developer. Stack: Spring Boot 3, MongoDB, JUnit 5. Rules: Follow existing patterns, Use @Valid on DTOs, Run tests before committing, No raw MongoDB queries -- use MongoTemplate" | ~200 | \$0.012 |
| **Bad (bloated)** | "You are a highly skilled and experienced senior software engineer specializing in Java development with deep expertise in Spring Boot, microservices architecture, MongoDB database design, and modern DevOps practices. You have 15 years of experience building enterprise-grade applications and are known for writing clean, maintainable, and well-tested code. When reviewing code, you should consider... \[continues for 1,800 more tokens\]" | ~2,000 | \$0.120 |
| **Savings** |  | **90%** | **\$0.108/session** |

### Move Specialized Content Out

|  |  |  |
|----|----|----|
| Content | Where | Loaded When |
| Core rules (10-20 lines) | System prompt / CLAUDE.md / GEMINI.md | Every message (cached) |
| Workflow procedures | Skills / separate files | On invocation only |
| API documentation | knowledge-base/ directory | When read by tool |
| Deployment guide | docs/deployment.md | When deploying |
| PR review checklist | /review-pr skill | During PR review |

#### Instruction Referencing

Define reusable instruction blocks once:

```
<instruction_set id="code_review">
Check for: SQL injection, XSS, CSRF, auth bypass, input validation
Severity: HIGH/MEDIUM/LOW
Format: issue, location, fix
</instruction_set>

For each file, apply instruction_set:code_review.
```

Registers the instruction once; references it as a single identifier in subsequent uses.

## Claude-Specific Techniques

### XML Tag Best Practices

```
<!-- Separate data from instructions -->
<documents>
  <document index="1">
    <source>report.pdf</source>
    <document_content>{{CONTENT}}</document_content>
  </document>
</documents>
<!-- Place long content FIRST, query LAST (+30% quality) -->
<data>{{LARGE_DOCUMENT}}</data>
<task>Summarize the key findings and recommend actions.</task>
```

### Ground Responses in Quotes

For long documents, ask Claude to quote first:

```
"Find relevant quotes from the documents, place them in <quotes> tags.
Then answer the question based only on those quotes, in <answer> tags."
```

This forces focused reading and prevents hallucination on large contexts.

### Control Verbosity

Claude 4.6 is naturally more concise. Steer format with positive instructions:

|  |  |  |
|----|----|----|
| Less Effective (negative) | More Effective (positive) | Why |
| "Do not use markdown in your response" | "Write in flowing prose paragraphs." | Tells Claude what TO do |
| "Don't include unnecessary details" | "Include only the 3 most critical findings." | Specific constraint |
| "Don't start with 'Based on...'" | "Respond with the answer only. No preamble, no caveats." | Direct instruction |
| "Don't use bullet points" | "Use numbered paragraphs with topic sentences." | Provides alternative format |

#### Use Effort Parameter (Claude 4.6)

|  |  |  |  |
|----|----|----|----|
| Task | Effort | Thinking Tokens | Cost Impact |
| Simple classification | `low` | Minimal | Cheapest |
| Code generation | `medium` | Moderate | Balanced |
| Complex architecture | `high` | Deep | Most expensive |
| Quick Q&A | `low` + thinking disabled | None | Minimal |

#### Parallel Tool Calls

```
"Read all 5 files in parallel, then analyze them together."
```

- Claude 4.6 aggressively parallelizes tool calls.

- Saves turns (and thus input token repetition) by reading multiple files at once.

## Gemini-Specific Techniques

### Leverage 1M Context Wisely

Even with 1M tokens, loading everything is wasteful:

|               |                                      |               |           |
|---------------|--------------------------------------|---------------|-----------|
| Approach      | Command                              | Tokens Loaded | Savings   |
| **Wasteful**  | `@./src/` (entire source tree)       | 500K          | Baseline  |
| **Efficient** | `@./src/auth/login.ts` (single file) | 2K            | **99.6%** |

### GEMINI.md Hierarchy

Use subdirectory overrides instead of one massive file:

```
~/.gemini/GEMINI.md          # 20 lines: global rules
./GEMINI.md                  # 30 lines: project overview
./src/api/GEMINI.md          # 15 lines: API rules only
./tests/GEMINI.md            # 10 lines: test rules only
```

Only relevant files load based on working directory.

### Save Before Compress

```
/memory add Decision: use PostgreSQL JSONB for config storage
/memory add Bug fix: auth race condition was missing DB lock
/compress
```

Memory entries survive compression; conversation details don't.

### Use /stats to Monitor

```
/stats    # Token usage, cache savings, cost estimate
```

## Tokenizer-Aware Writing

### Phrasing & Data Format Token Comparison

Different phrasings and data formats produce vastly different token counts for identical meaning:

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| Category | Verbose | Tokens | Compact | Tokens | Savings |
| **Phrasing** | "make use of" | 3 | "use" | 1 | 67% |
| **Phrasing** | "more or less" | 3 | "about" | 1 | 67% |
| **Phrasing** | "due to the fact that" | 5 | "because" | 1 | 80% |
| **Phrasing** | "in order to" | 3 | "to" | 1 | 67% |
| **Phrasing** | "at this point in time" | 5 | "now" | 1 | 80% |
| **Phrasing** | "New York City" | 3 | "NYC" | 1 | 67% |
| **Phrasing** | "one thousand" | 2 | "1000" | 1 | 50% |
| **Data format** | Prose description | ~200 | CSV | ~50 | 75% |
| **Data format** | JSON (verbose keys) | ~120 | JSON (short keys) | ~80 | 33% |
| **Data format** | JSON (short keys) | ~80 | YAML | ~70 | 13% |
| **Data format** | YAML | ~70 | CSV | ~50 | 29% |
| **Data format** | CSV | ~50 | TOON (schema-aware) | ~48 | 4% |

### Practical Rules

1.  **Use common words** -- "use" not "utilize", "about" not "approximately"

2.  **Avoid redundant modifiers** -- "very unique" = "unique"

3.  **Prefer single words** -- "because" not "due to the fact that"

4.  **Use standard abbreviations** -- "API", "DB", "URL" are single tokens

5.  **Compact numbers** -- "1000" not "one thousand"

6.  **Remove unnecessary formatting** -- no decorative separators, ASCII art

## Measurement & Monitoring

### Track Token Usage

**Claude Code:**

```
/cost     # Shows token breakdown and costs (API users)
/context  # Shows what's consuming context space
/stats    # Usage patterns (subscribers)
```

**Gemini CLI:**

```
/stats    # Token usage, cached tokens, cost estimate
```

#### Estimate Tokens

```
# Quick estimation (English text)
def estimate_tokens(text):
    return len(text.split()) * 1.3  # ~1.3 tokens per word

# Accurate count (Claude)
import anthropic
count = client.messages.count_tokens(
    model="claude-sonnet-4-20250514",
    messages=[{"role": "user", "content": text}],
)
print(f"Exact tokens: {count.input_tokens}")
```
