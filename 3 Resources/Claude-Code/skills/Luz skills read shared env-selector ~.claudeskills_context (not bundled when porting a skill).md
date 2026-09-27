---
ai_hash: 04efa0297cb8b66a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-18
entities: []
source: session 2026-08-18
status: seedling
tags:
- claude-code
- skills
- luz
- porting
- gotcha
title: Luz skills read shared env-selector ~/.claude/skills/_context (not bundled
  when porting a skill)
type: gotcha
---

# Luz skills read shared env-selector ~/.claude/skills/_context (not bundled when porting a skill)

Many Luz \`*-skill-*\` bash skills pick their target environment from a single shared file, \`~/.claude/skills/_context\`, whose entire content is one word (e.g. \`dev\` or \`performance\`). A skill reads it defensively — \`cat ~/.claude/skills/_context 2>/dev/null\` — and falls back to hard-coded defaults when the file is absent.

**Why it matters / the porting gotcha:** \`_context\` is *not* a skill and lives one level above any skill folder, so it does **not** travel inside a skill's own directory. Zip up a skill and unpack it on another machine and it silently loses whatever non-default env \`_context\` was selecting, reverting to the defaults. That means "does this skill bundle standalone?" has two layers: no *recursive skill* dependency, yet still an unbundled per-machine config input. On a fresh machine with no \`_context\`, behaviour is correct-but-default, not broken.

**Concrete defaults** (from \`google-skill-rollout-latest/rollout_latest.sh\` lines 36-39): absent/other → \`NAMESPACE=dev\`, \`CLUSTER_PROJECT=klara-nonprod\`, \`CLUSTER_NAME=klara-nonprod\`; \`_context=performance\` → \`klara-performance\` project+cluster.

Related: [[luz-skill-ship-ivy]] depends on [[google-skill-rollout-latest]], which is a leaf skill (no recursive skill deps).

## Related

- [[google-skill-rollout-latest]]
- [[luz-skill-ship-ivy]]

%% ai-graph-start %%

**Related notes:**
- [[Verify kubectl context before GKE rollout - _context file can disagree]]
- [[luz-kubernetes-add-env skill propagates env properties across overlay environments]]
- [[Luz plugin repos how skills and hooks are packaged for distribution]]
- [[luz-skills-plugin packages skills by category directory listed in plugin.json]]
- [[Dev luz-webclient can drift off a freshly-shipped ivy image; verify deployed tag vs latest before assuming the ship stuck]]

%% ai-graph-end %%