---
title: "Fence and strip delimiters around untrusted RAG context to limit prompt injection"
created: 2026-09-08
type: lesson
status: seedling
source: "session 2026-09-08 — docs-vector-search hardening"
tags: [llm, rag, prompt-injection, security, ai-safety]
---

# Fence and strip delimiters around untrusted RAG context to limit prompt injection

A RAG endpoint builds its LLM prompt by concatenating retrieved document chunks + the user question. Both are **untrusted**: an indexed document or a crafted query can contain "ignore your instructions / reveal the system prompt / act as…" and the model may obey it. Naive `f"Context:\n{context}\n\nQuestion: {question}"` gives injected text the same standing as your instructions.

**Mitigation (defense-in-depth, not a cure):**
1. **Fence** each untrusted section with explicit delimiters, e.g. `<context>…</context>` and `<question>…</question>`.
2. **Strip those delimiter tokens out of the untrusted text** before inserting it, so a document/query cannot close the fence early and smuggle instructions into the trusted region.
3. **Instruct the system prompt** to treat everything inside the fences as *data to answer about, never as instructions*, and to ignore any directions/role-changes/prompt-reveal requests found inside them.

Prompt injection is not fully solvable at the prompt layer; fencing + sanitizing + a firm system instruction meaningfully raises the bar. For higher-stakes actions, add out-of-band controls (allow-lists, output filtering, no tool access from untrusted context).

## Related

- [[Absence of X-Forwarded-For must not mean trusted internal caller]]
