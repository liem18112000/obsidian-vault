---
ai_hash: 38051e45248bce2a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-10
entities:
- luz-env-config-reminder hook
- overlay propagation
- luz repos
- PostToolUse hook
- ~/.claude/hooks/luz/luz-env-config-reminder.ps1
- settings.json
- Edit|Write|MultiEdit matcher
- env read
- env config
- System.getenv("X")
- '@ConfigProperty(name="X")'
- MicroProfile
- .getValue("X")
- .getOptionalValue("X")
- KEY=
- 'KEY:'
- .properties files
- .env files
- docker-compose files
- configmap files
- additionalContext
- Claude
- luz-kubernetes-add-env skill
- overlay environments
- luz_kubernetes
- kubernetes-overlays
- .git
- node_modules
- target
- build
- repo segment
- dashed overlay name
- SERVICE hint
- luz_docs
- luz-docs
- luz_kubernetes overlay layout system.properties per env-envservice
- detected variables
- luz- segment
- luz_ segment
- env properties
source: session 2026-06-10
status: seedling
tags:
- claude-code
- hook
- luz
- env-config
title: luz-env-config-reminder hook nudges overlay propagation for new env reads in
  luz repos
type: howto
---

# luz-env-config-reminder hook nudges overlay propagation for new env reads in luz repos

PostToolUse hook `~/.claude/hooks/luz/luz-env-config-reminder.ps1` (registered in `settings.json` under the `Edit|Write|MultiEdit` matcher) watches edits inside any repo whose path contains a `luz-`/`luz_` segment (luz_docs, luz-store, luz-enrichment, …).

When the added text introduces an env read or env config — `System.getenv("X")`, `@ConfigProperty(name="X")`, MicroProfile `.getValue("X")`/`.getOptionalValue("X")`, or new `KEY=`/`KEY:` lines in `.properties`/`.env`/docker-compose/configmap files — it injects additionalContext naming the detected variables and telling Claude to offer the [[luz-kubernetes-add-env skill propagates env properties across overlay environments]] so the var also lands in the 7 overlay environments.

Deliberately silent inside `luz_kubernetes` itself / `kubernetes-overlays` paths (that's the skill's target, not a source) and in `.git`, `node_modules`, `target`, `build`. It normalizes the repo segment to a dashed overlay name for the SERVICE hint (`luz_docs` → `luz-docs`).

Only var-shaped names fire: `[A-Z][A-Z0-9_]{2,}` — lowercase config keys won't trigger it.

## Related

- [[luz-kubernetes-add-env skill propagates env properties across overlay environments]]
- [[luz_kubernetes overlay layout system.properties per env-envservice]]

%% ai-graph-start %%

**Related notes:**
- [[luz-kubernetes-add-env skill propagates env properties across overlay environments]]
- [[Luz plugin repos how skills and hooks are packaged for distribution]]
- [[luz_kubernetes overlay layout system.properties per env-envservice]]
- [[luz-hooks-plugin packages each hook as its own plugin registered in marketplace.json]]
- [[luz-skills-plugin packages skills by category directory listed in plugin.json]]

**Relations:**
- luz-env-config-reminder hook — *nudges* — overlay propagation
- luz-env-config-reminder hook — *monitors* — luz repos
- luz-env-config-reminder hook — *is a* — PostToolUse hook
- luz-env-config-reminder hook — *path* — ~/.claude/hooks/luz/luz-env-config-reminder.ps1
- luz-env-config-reminder hook — *registered in* — settings.json
- settings.json — *uses* — Edit|Write|MultiEdit matcher
- luz-env-config-reminder hook — *detects* — env read
- luz-env-config-reminder hook — *detects* — env config
- env read — *example* — System.getenv("X")
- env config — *example* — @ConfigProperty(name="X")
- env config — *example* — MicroProfile .getValue("X")
- env config — *example* — MicroProfile .getOptionalValue("X")
- env config — *example* — KEY=
- env config — *example* — KEY:
- KEY= — *found in* — .properties files
- KEY: — *found in* — .properties files
- KEY= — *found in* — .env files
- KEY: — *found in* — .env files
- KEY= — *found in* — docker-compose files
- KEY: — *found in* — docker-compose files
- KEY= — *found in* — configmap files
- KEY: — *found in* — configmap files
- luz-env-config-reminder hook — *injects* — additionalContext
- additionalContext — *names* — detected variables
- additionalContext — *prompts* — Claude
- Claude — *offers* — luz-kubernetes-add-env skill
- luz-kubernetes-add-env skill — *propagates* — env properties
- env properties — *across* — overlay environments
- overlay environments — *number* — 7
- luz-env-config-reminder hook — *is silent in* — luz_kubernetes
- luz-env-config-reminder hook — *is silent in* — kubernetes-overlays
- luz_kubernetes — *is target for* — luz-kubernetes-add-env skill
- kubernetes-overlays — *is target for* — luz-kubernetes-add-env skill
- luz-env-config-reminder hook — *ignores* — .git
- luz-env-config-reminder hook — *ignores* — node_modules
- luz-env-config-reminder hook — *ignores* — target
- luz-env-config-reminder hook — *ignores* — build
- luz-env-config-reminder hook — *normalizes* — repo segment
- repo segment — *to* — dashed overlay name
- dashed overlay name — *used for* — SERVICE hint
- luz_docs — *normalizes to* — luz-docs
- luz-env-config-reminder hook — *related to* — luz-kubernetes-add-env skill
- luz-env-config-reminder hook — *related to* — luz_kubernetes overlay layout system.properties per env-envservice
- luz repos — *contains segment* — luz-
- luz repos — *contains segment* — luz_

%% ai-graph-end %%