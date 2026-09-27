---
ai_hash: fcb295b2270e48cd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities:
- Enabled thinking
- '`max_tokens`'
- output truncation
- Claude request
- thinking tokens
- visible response text
- model
- '`stop_reason: "max_tokens"`'
- legacy code
- generation calls
- summary
- JSON payload
- model upgrade
- Sonnet 5
- Opus 5
- '`thinking` parameter'
- '`thinking={"type": "disabled"}`'
- raising `max_tokens` value
- '`Anthropic SDK message content[0].text breaks on Claude 5 thinking blocks`'
source: session 2026-08-28 (KGA refine fix)
status: seedling
tags:
- anthropic
- claude
- llm
- max-tokens
- gotcha
title: Enabled thinking shares the max_tokens budget and can truncate output
type: lesson
---

# Enabled thinking shares the max_tokens budget and can truncate output

When extended/adaptive thinking is enabled on a Claude request, **thinking tokens count against `max_tokens`** — `max_tokens` is a hard cap on thinking **plus** the visible response text, not just the answer. On a tight budget the model can spend most of it thinking and then truncate the actual output (or hit `stop_reason: "max_tokens"`).

This bites code written **before** thinking existed: short generation calls with small caps (e.g. `max_tokens=700` for a summary, `1500` for a JSON payload) were sized assuming the whole budget was output. After a model upgrade where thinking is on by default (Sonnet 5 / Opus 5 when `thinking` is omitted), the same call can return truncated or empty output.

**Fix:** for tight-budget generation calls either pass `thinking={"type": "disabled"}` to reclaim the full budget for output, or raise `max_tokens` to leave room for thinking. Relates to [[Anthropic SDK message content[0].text breaks on Claude 5 thinking blocks]].

## Related

- [[Anthropic SDK message content[0].text breaks on Claude 5 thinking blocks]]

%% ai-graph-start %%

**Related notes:**
- [[Anthropic SDK message content[0].text breaks on Claude 5 thinking blocks]]
- [[OpenAI request gotchas 8192-token embedding limit and max_completion_tokens]]
- [[Fix TPD scenario generator truncation — raise max_tokens, keep one call]]
- [[LLM-as-reranker JSON truncation budget max_tokens for pretty-printed output, not just element count]]
- [[Why local test-agent is slow claude -p ships a 17.5K agent prompt on Opus, x many serial calls]]

**Relations:**
- Enabled thinking — *shares budget with* — `max_tokens`
- Enabled thinking — *can cause* — output truncation
- thinking tokens — *count against* — `max_tokens`
- `max_tokens` — *caps* — thinking tokens
- `max_tokens` — *caps* — visible response text
- model — *can cause* — output truncation
- model — *can trigger* — `stop_reason: "max_tokens"`
- legacy code — *assumed budget for* — output
- generation calls — *use* — `max_tokens`
- `max_tokens=700` — *is for* — summary
- `max_tokens=1500` — *is for* — JSON payload
- model upgrade — *enables by default* — Enabled thinking
- Sonnet 5 — *is a type of* — model upgrade
- Opus 5 — *is a type of* — model upgrade
- Sonnet 5 — *enables by default* — Enabled thinking
- Opus 5 — *enables by default* — Enabled thinking
- omitting `thinking` parameter — *enables by default* — Enabled thinking
- tight-budget generation calls — *fixed by* — `thinking={"type": "disabled"}`
- tight-budget generation calls — *fixed by* — raising `max_tokens` value
- Fix — *relates to* — `Anthropic SDK message content[0].text breaks on Claude 5 thinking blocks`
- `Anthropic SDK message content[0].text breaks on Claude 5 thinking blocks` — *is related to* — Enabled thinking

%% ai-graph-end %%