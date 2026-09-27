---
ai_hash: 7f703c36e4093f68
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-25
entities: []
source: session 2026-09-25
status: seedling
tags:
- protocols
- parsing
- gotcha
- api-design
title: A greedy partition marker swallows parameters emitted after it
type: lesson
---

# A greedy partition marker swallows parameters emitted after it

If a parser splits a message on a marker and takes everything after it, any parameter emitted AFTER that marker is swallowed into the previous one.

The parse was a plain greedy split:

```python
head, _, tail = text.partition("guidance:")   # tail = the free-text steer
```

Anything appended after the `guidance:` line lands inside `tail` and becomes part of the steer text — silently. Adding a new `rigor: 3` parameter to the end of the message would have made it invisible to the reader and quietly corrupted the steer.

**Rules:**
1. New parameters go BEFORE any greedy free-text marker. Structured fields first, the open-ended one last.
2. Put the constraint in a comment on BOTH sides — the emitter and the parser — because neither is wrong on its own and a reader of either file cannot see the coupling.
3. Test the ordering contract by asserting the BROKEN order does not parse:

```python
bad_head, _, _ = "cmd\nguidance: go\nrigor: 3".partition("guidance:")
assert re.search(r"rigor:\s*(\d+)", bad_head) is None
```

That negative assertion is what stops a future contributor from appending the next field to the end of the message, which is the natural thing to do.

General shape: any protocol with exactly one free-text/greedy field must place it last and say so. Same class of bug as putting a variadic argument before a keyword one.

Related: [[Check every stage that writes a field, not just the one that defines it]]

## Related

- [[Check every stage that writes a field, not just the one that defines it]]

%% ai-graph-start %%

**Related notes:**
- _(none above threshold)_

%% ai-graph-end %%