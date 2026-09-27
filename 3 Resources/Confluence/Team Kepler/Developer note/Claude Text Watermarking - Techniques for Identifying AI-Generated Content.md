---
title: "Claude Text Watermarking: Techniques for Identifying AI-Generated Content"
created: 2026-08-17
updated: 2026-08-17
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49670750212/Claude+Text+Watermarking+Techniques+for+Identifying+AI-Generated+Content
confluence_id: "49670750212"
confluence_path: "Team Kepler > Developer note"
tags: [confluence]
---

# Claude Text Watermarking: Techniques for Identifying AI-Generated Content

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49670750212/Claude+Text+Watermarking+Techniques+for+Identifying+AI-Generated+Content) · updated 2026-08-17*

## Why

Per Anthropic's announcement: driven by **EU AI Act** transparency obligations, with a global rollout targeted for **August 2026**; older Claude models to be watermarked over the following months. The watermark **carries no identifying information** — not traceable to a person, org, or chat.

## What

Claude picks the next word from among **equally‑good** options using a **key** (plus the words so far) instead of an arbitrary coin flip. Do this for every word and the choices leave a faint, reproducible pattern — invisible to a reader, measurable with the key.

### Glossary

- **SynthID‑Text** — DeepMind's production text‑watermark (Nature 2024); descends from Scott Aaronson's 2022 proposal.

- **g‑function / g‑value** — pseudo‑random function seeded by key + preceding‑token context; scores each candidate token.

- **Tournament sampling** — candidate tokens compete in bracket rounds; higher g‑values advance; winner is emitted.

- `ngram_len` — preceding‑token context window feeding the g‑function; larger → more detectable.

- **Non‑distortionary** — preserves the model's output distribution (and thus quality) in expectation.

- **Mean g‑value** — the detection score: average of recomputed g‑values.

- **C2PA / Content Credentials** — open provenance standard / its user‑facing name.

- **Manifest · Assertion · Claim · Claim Signature** — the signed provenance record and its parts.

- **Hard vs soft binding** — cryptographic byte‑hash (exact, fragile) vs watermark/fingerprint (robust, re‑matchable).

- **Durable Content Credentials** — provenance recoverable via soft bindings after the manifest is stripped.

![[image-20260817-061735.png]]

### References

1.  **Anthropic** — *Claude's text watermark.* [https://www.anthropic.com/news/claude-text-watermark](https://www.anthropic.com/news/claude-text-watermark)

2.  **Nature (Dathathri et al., DeepMind, 2024)** — *Scalable watermarking for identifying LLM outputs* (SynthID‑Text). [https://www.nature.com/articles/s41586-024-08025-4](https://www.nature.com/articles/s41586-024-08025-4)

3.  **C2PA** — *Specification v2.3.* [https://spec.c2pa.org/specifications/specifications/2.3/specs/C2PA_Specification.html](https://spec.c2pa.org/specifications/specifications/2.3/specs/C2PA_Specification.html)

## Two techniques, by content type

- Text has thousands of low‑stakes word choices to hide a signal in; a file's bytes don't.

- So Claude **embeds** at generation, then **detects/verifies** afterward — split by content type.

![[image-20260817-061858.png]]

- **Watermarked** where Claude freely chooses words (prose, creative, code comments, translations).

- **Not** where words are fixed: executable code / exact syntax, precise facts, or light proofreading.

![[image-20260817-062029.png]]

## Text — embedding

### Tournament sampling

Seed a pseudo‑random **g‑function** with the **key + preceding tokens** (`ngram_len`); candidate tokens compete in a **tournament**; the higher **g‑value** wins and is emitted. Repeat per token.

![[image-20260817-062223.png]]

> [!note]- Expand for more mathematics example
>
> ![[image-20260817-063510.png]]
>

### **Zoom in — how a g‑value is computed and used:**

- **Flow:** context → model distribution → g‑function → tournament → emit → repeat.

- **Non‑distortionary:** over many tokens the output distribution matches the model's own — the key only picks *which* equally‑good token wins, never a *worse* one. No quality/creativity loss.

- **Free:** no extra tokens → no speed/cost impact; composes with speculative sampling.

- **Knobs:** `keys` (~20–30 random ints) · `ngram_len` (bigger → more detectable).

![[image-20260817-062313.png]]

> [!note]- Expand for more mathematics example
>
> ![[image-20260817-063539.png]]
>

### Detecting (and its limits)

Replay the computation: recompute each token's g‑value with the same key, **average** them (**mean g‑value**), and compare to a **threshold**. Unwatermarked text averages ≈ 0.5; watermarked skews higher.

- **Result:** a **confidence score**, not a yes/no — e.g. 99.9%. Threshold is tunable (higher → fewer false positives).

- **Weakens / fails on:** short text · highly factual low‑entropy answers · heavy editing or full rewrite (removes it) · translation/paraphrase by another system.

- **Cannot:** identify *which* AI, or confirm *human* authorship.

> [!note]- Detection performance of SynthID-Text.
>
> ![[image-20260817-063802.png]]
>

![[image-20260817-062403.png]]

## Files

### C2PA content credentials

For non‑text output Claude attaches a **manifest**: signed statements binding the file to its history.

- **Anatomy:**

  - *Assertions* (metadata, actions, content hash, ingredients) → *Claim* (references them, CBOR + hash) → *Claim Signature* (signs with an X.509 private key).

  - Every edit **appends** a manifest → full provenance chain.

<!-- -->

- **Verify:**

  - Signature + certificate (trust list) + hashes → *who* made it, *what* changed, whether it was tampered with.

- **Hard binding** = hash of exact bytes (tamper‑evident, lost on re‑encode).

- **Soft binding** = watermark/fingerprint (survives format changes → re‑matches a stripped file).

  - Soft bindings enable **Durable Content Credentials** — the same watermarking idea, applied to files.

![[image-20260817-062638.png]]
