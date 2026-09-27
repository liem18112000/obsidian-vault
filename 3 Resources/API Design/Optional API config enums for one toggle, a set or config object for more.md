---
ai_hash: 4a6dfbbf2be07316
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Java''s options for options (TS, reposting Ethan McCue)'
status: seedling
tags:
- api-design
- java
- method-signatures
- builder-pattern
- enumset
- confluence-distilled
title: 'Optional API config: enums for one toggle, a set or config object for more'
type: lesson
---

# Optional API config: enums for one toggle, a set or config object for more

Ethan McCue's "Java's options for options" walks a single question — *how do I let callers configure this?* — through **thirteen** designs, using a JSON writer that needs an indentation toggle. The value is not any one option but the **order they stop working in** as the number of toggles grows.

**One option, and it might stay one:**

1. **Don't support it.** Listed first deliberately: *"any toggles you add to your API are toggles you might need to support now and forever more."* A narrower API is a real choice, not a cop-out.
2. **A second method** — `writeJson` / `writeJsonWithIndentation`. Perfectly readable for exactly one toggle.
3. **A boolean argument** — `writeJson(out, json, true)`. Already bad: the call site tells you nothing about what `true` means.
4. **An enum argument** — `writeJson(out, json, Indentation.INDENT)`. Same shape as 3, readable at the call site, and extensible to a third mode later.

**Then a second option appears, and the wheels come off:**

5–6. **A method per combination.** Two toggles is four methods; each new toggle *doubles* the surface. Option 13 makes it concrete — adding one more capability forces **four more methods**.
7. **Two boolean arguments** — `writeJson(out, json, true, false)` is now a positional puzzle, and swapping the two arguments compiles silently.
8. **Two enum arguments** — readable, but the signature grows one parameter per option forever.
9–10. **Bit flags, then `EnumSet`.** Both move from "N parameters" to "a set of requested behaviours", so adding an option does not change the signature. `EnumSet` is the type-safe modern form; raw bit flags are the C-era ancestor.
11–12. **A config object**, transparent or opaque. The distinction matters most: an **opaque** object (constructed only through a builder, fields not part of the contract) lets you add options later without breaking any caller. A **transparent** one exposes its shape and freezes it.

> [!tip] The inflection point
> **One** option → a named method or an enum. **Two or more, or any chance of more later** → jump straight to a set or a config object. The intermediate designs (boolean parameters, method-per-combination) are the ones that feel fine at the moment you write them and become unmaintainable on the next feature.

> [!warning] Booleans at a call site are write-only code
> `writeJson(out, json, true, false)` requires opening the signature to read. Enums, `EnumSet`, and builders all cost a few more characters and remove that lookup permanently. This is the single highest-value swap in the whole list.

Related: [[Mutually exclusive API parameters should be rejected, not resolved by precedence]] — the other half of designing a parameter list.

Source: [[Java's options for options]] (TS, Confluence — reposting Ethan McCue).

## Related

- [[Mutually exclusive API parameters should be rejected, not resolved by precedence]]

%% ai-graph-start %%

**Related notes:**
- [[Java's options for options]]

%% ai-graph-end %%