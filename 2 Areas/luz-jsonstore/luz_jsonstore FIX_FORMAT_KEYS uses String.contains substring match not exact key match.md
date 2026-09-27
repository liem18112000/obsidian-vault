---
ai_hash: 965521b542a45033
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities:
- luz_jsonstore
- FIX_FORMAT_KEYS
- String.contains
- exact key match
- tempAdjustFormat
- Constants.CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS
- String
- field name
- int-coerce
- false-positive source
- System.getenv("CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS")
- environment variable
- 'null'
- CH_KLARA_JSONSTORE_FIX_FORMAT
- luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read
- this note
source: session 2026-08-17
status: seedling
tags:
- luz-jsonstore
- gotcha
- config
title: luz_jsonstore FIX_FORMAT_KEYS uses String.contains substring match not exact
  key match
type: lesson
---

# luz_jsonstore FIX_FORMAT_KEYS uses String.contains substring match not exact key match

The field-selection guard in \`tempAdjustFormat\` is \`Constants.CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS.contains(key)\` — and \`CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS\` is a plain \`String\`, so this is a \`String.contains\` **substring** test, not set/exact membership.

**Consequence:** any field name that happens to be a substring of the configured keys string matches. E.g. with keys configured as \`"accountNumber"\`, a field literally named \`"count"\` or \`"Number"\` would match and get coerced. This is a latent false-positive source when picking which fields to int-coerce.

**Extra trap:** the constant is built as \`"" + System.getenv("CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS")\`, so when the env var is unset the string is the literal \`"null"\` — meaning fields named \`n\`, \`u\`, \`l\`, \`nu\`, \`null\`, etc. would technically match (the master \`CH_KLARA_JSONSTORE_FIX_FORMAT\` switch normally gates this, but the two flags are independent).

Part of [[luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read]].

## Related

- [[luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read]]

%% ai-graph-start %%

**Related notes:**
- [[luz_jsonstore tempAdjustFormat overwrites whole list when targeted key holds a numeric array]]
- [[luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read]]
- [[jsonstore $in vs $nin ObjectId conversion gap]]
- [[luz_jsonstore find projectsortcollation params must omit outer braces (server wraps them)]]
- [[transaction_status column stores Java enum names not JSON wire values]]

**Relations:**
- luz_jsonstore — *uses* — FIX_FORMAT_KEYS
- FIX_FORMAT_KEYS — *implements* — String.contains
- String.contains — *is not* — exact key match
- tempAdjustFormat — *uses guard* — Constants.CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS
- Constants.CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS — *is a* — String
- String.contains — *causes* — field name
- field name — *to match if substring of* — Constants.CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS
- field name — *match leads to* — int-coerce
- int-coerce — *is a* — false-positive source
- Constants.CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS — *is built from* — System.getenv("CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS")
- environment variable — *being unset results in* — null
- null — *causes match for* — field name
- CH_KLARA_JSONSTORE_FIX_FORMAT — *gates* — null
- FIX_FORMAT_KEYS — *and* — CH_KLARA_JSONSTORE_FIX_FORMAT
- FIX_FORMAT_KEYS and CH_KLARA_JSONSTORE_FIX_FORMAT — *are* — independent
- this note — *is part of* — luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read
- this note — *is related to* — luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read

%% ai-graph-end %%