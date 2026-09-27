---
ai_hash: fd19e5e3bf7f706f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities:
- luz_jsonstore tempAdjustFormat
- JsonStoreMongoDbService
- Document
- Double
- Integer
- JSON
- Constants.CH_KLARA_JSONSTORE_FIX_FORMAT
- CH_KLARA_JSONSTORE_FIX_FORMAT
- convert()
- Constants.java
- getOne
- getMany
- aggregate
- findOneAndUpdate
- luz_jsonstore FIX_FORMAT_KEYS uses String.contains substring match not exact key
  match
- luz_jsonstore tempAdjustFormat overwrites whole list when targeted key holds a numeric
  array
- results
- stopgap
source: session 2026-08-17
status: seedling
tags:
- luz-jsonstore
- mongodb
- bson
- data-normalization
title: luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read
type: howto
---

# luz_jsonstore tempAdjustFormat coerces Double fields to Integer on read

In \`JsonStoreMongoDbService\`, \`tempAdjustFormat(Document)\` is a recursive, in-place normalization pass applied to results just before they are returned to callers — it runs in \`getOne\`, \`getMany\`, \`aggregate\`, and \`findOneAndUpdate\`. Its only real job is to coerce values that deserialized as \`Double\` back into \`Integer\` for a configured set of field names (a band-aid for JSON having no int type, so `5` came back as `5.0`).

**Env-gated no-op by default.** The entire body is skipped unless \`Constants.CH_KLARA_JSONSTORE_FIX_FORMAT\` is true, which is true only when the env var \`CH_KLARA_JSONSTORE_FIX_FORMAT\` is set to anything (even empty). In any env without that var, the method does nothing.

**Truncates, does not round.** The helper \`convert()\` does \`((Double)value).intValue()\`, which truncates toward zero rather than rounding: \`5.0->5\`, \`5.9->5\`, \`-1.7->-1\`.

Marked \`// TEMP\` in \`Constants.java\` — intended as a stopgap. See related gotchas: [[luz_jsonstore FIX_FORMAT_KEYS uses String.contains substring match not exact key match]] and [[luz_jsonstore tempAdjustFormat overwrites whole list when targeted key holds a numeric array]].

## Related

- [[luz_jsonstore FIX_FORMAT_KEYS uses String.contains substring match not exact key match]]
- [[luz_jsonstore tempAdjustFormat overwrites whole list when targeted key holds a numeric array]]

%% ai-graph-start %%

**Related notes:**
- [[luz_jsonstore tempAdjustFormat overwrites whole list when targeted key holds a numeric array]]
- [[luz_jsonstore FIX_FORMAT_KEYS uses String.contains substring match not exact key match]]
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]
- [[luz_jsonstore committed V2 updateOne count delete have latent BSON serialization bug]]
- [[luz_jsonstore find projectsortcollation params must omit outer braces (server wraps them)]]

**Relations:**
- luz_jsonstore tempAdjustFormat — *is_method_of* — JsonStoreMongoDbService
- luz_jsonstore tempAdjustFormat — *takes_parameter* — Document
- luz_jsonstore tempAdjustFormat — *coerces_type_from* — Double
- luz_jsonstore tempAdjustFormat — *coerces_type_to* — Integer
- luz_jsonstore tempAdjustFormat — *is_applied_to* — results
- luz_jsonstore tempAdjustFormat — *executes_in* — getOne
- luz_jsonstore tempAdjustFormat — *executes_in* — getMany
- luz_jsonstore tempAdjustFormat — *executes_in* — aggregate
- luz_jsonstore tempAdjustFormat — *executes_in* — findOneAndUpdate
- JSON — *lacks_data_type* — int
- luz_jsonstore tempAdjustFormat — *is_gated_by* — Constants.CH_KLARA_JSONSTORE_FIX_FORMAT
- Constants.CH_KLARA_JSONSTORE_FIX_FORMAT — *is_controlled_by_env_var* — CH_KLARA_JSONSTORE_FIX_FORMAT
- luz_jsonstore tempAdjustFormat — *uses_helper_method* — convert()
- convert() — *truncates_type* — Double
- convert() — *to_type* — Integer
- luz_jsonstore tempAdjustFormat — *is_marked_in_file* — Constants.java
- luz_jsonstore tempAdjustFormat — *is_intended_as* — stopgap
- luz_jsonstore tempAdjustFormat — *has_related_issue* — luz_jsonstore FIX_FORMAT_KEYS uses String.contains substring match not exact key match
- luz_jsonstore tempAdjustFormat — *has_related_issue* — luz_jsonstore tempAdjustFormat overwrites whole list when targeted key holds a numeric array

%% ai-graph-end %%