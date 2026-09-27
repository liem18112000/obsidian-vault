---
ai_hash: de6b35f8bc1c2c98
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities:
- luz_jsonstore tempAdjustFormat
- List
- Double
- Integer
- CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS
- document.put(key, convert(key, item))
- list.set(i, convert(...))
- luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read
- numeric array
- scalar element
- list key
- whole list
- single Integer
- latent bug
- List<Double>
- lone integer
- list element by index
source: session 2026-08-17
status: seedling
tags:
- luz-jsonstore
- bug
- gotcha
title: luz_jsonstore tempAdjustFormat overwrites whole list when targeted key holds
  a numeric array
type: lesson
---

# luz_jsonstore tempAdjustFormat overwrites whole list when targeted key holds a numeric array

In \`tempAdjustFormat\`, when a value is a \`List\` the code iterates its elements, and for a scalar element that is a targeted \`Double\` it executes \`document.put(key, convert(key, item))\`. That writes the converted scalar back under the **list key**, so it replaces the ENTIRE list with a single \`Integer\` (effectively the last converted element) instead of converting each element in place.

**Impact:** latent bug. It is currently harmless only because the targeted keys hold scalars, not numeric arrays. If any key in \`CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS\` ever points at a \`List<Double>\`, that field would be silently corrupted from an array into a lone integer on read.

**Correct fix would be** to mutate the list element by index (e.g. \`list.set(i, convert(...))\`) rather than \`document.put(key, ...)\`.

Part of [[luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read]].

## Related

- [[luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read]]

%% ai-graph-start %%

**Related notes:**
- [[luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read]]
- [[luz_jsonstore FIX_FORMAT_KEYS uses String.contains substring match not exact key match]]
- [[luz_jsonstore committed V2 updateOne count delete have latent BSON serialization bug]]
- [[luz_jsonstore silently drops _shard on $set updates (HTTP 200, no persist)]]
- [[json-patch-independent-translation-breaks-reset-then-append]]

**Relations:**
- luz_jsonstore tempAdjustFormat — *overwrites* — whole list
- luz_jsonstore tempAdjustFormat — *targets* — numeric array
- luz_jsonstore tempAdjustFormat — *iterates elements of* — List
- scalar element — *is a targeted* — Double
- document.put(key, convert(key, item)) — *writes converted scalar back under* — list key
- document.put(key, convert(key, item)) — *replaces* — whole list
- document.put(key, convert(key, item)) — *replaces with* — single Integer
- luz_jsonstore tempAdjustFormat — *has impact* — latent bug
- CH_KLARA_JSONSTORE_FIX_FORMAT_KEYS — *points at* — List<Double>
- List<Double> — *would be corrupted into* — lone integer
- list.set(i, convert(...)) — *mutates* — list element by index
- luz_jsonstore tempAdjustFormat — *is part of* — luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read
- luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read — *is related to* — luz_jsonstore tempAdjustFormat

%% ai-graph-end %%