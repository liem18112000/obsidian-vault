---
ai_hash: 6084abf224978073
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities: []
source: session 2026-08-28 (KGA refine fix)
status: seedling
tags:
- anthropic
- claude
- vertex-ai
- llm
- gotcha
title: Anthropic SDK message content[0].text breaks on Claude 5 thinking blocks
type: lesson
---

# Anthropic SDK message content[0].text breaks on Claude 5 thinking blocks

The Anthropic Python SDK returns a message whose `content` is a **list of typed blocks** (`TextBlock`, `ThinkingBlock`, `ToolUseBlock`, ...), not a string. Reading `msg.content[0].text` assumes the first block is text — but Claude 4.5+/5 models emit a `ThinkingBlock` first, and Sonnet 5 / Opus 5 run **adaptive thinking by default when the `thinking` param is omitted**. A `ThinkingBlock` has `.thinking`, not `.text`, so the access raises `AttributeError: 'ThinkingBlock' object has no attribute 'text'`.

This is sneaky because adaptive thinking is **content-dependent**: a trivial prompt (short "summarize in 2 sentences", tiny `max_tokens`) may not trigger thinking and the code "works", while a complex prompt emits a thinking block first and crashes — so the same line passes on easy calls and fails intermittently on hard ones. It typically surfaces right after bumping a model id to `claude-sonnet-5` / `claude-opus-5` with no other change (also applies to `AnthropicVertex` on Google Vertex AI).

**Fix:** never index `content[0]`. Walk the blocks and take the first text one:
```python
def first_text(msg):
    for b in msg.content:
        if getattr(b, "type", None) == "text":
            return b.text
    return ""
```
Optionally also pass `thinking={"type": "disabled"}` to restore the old no-thinking behaviour. See also [[Enabled thinking shares the max_tokens budget and can truncate output]].

## Related

- [[Enabled thinking shares the max_tokens budget and can truncate output]]

%% ai-graph-start %%

**Related notes:**
- [[Enabled thinking shares the max_tokens budget and can truncate output]]
- [[Claude on Vertex AI uses anthropic[vertex], not google-genai]]
- [[Claude Sonnet 5 confirmed working on Vertex AI for klara-nonprod]]
- [[Claude on Vertex AI availability is per-project per-region (klara-nonprod)]]
- [[Anthropic has no first-party embeddings endpoint]]

%% ai-graph-end %%