---
ai_hash: 9c070fd2d6df3c78
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: luz-hooks-plugin simplify-gate PR, 2026-09-27
status: seedling
tags:
- claude-code
- hooks
- concurrency
- gotcha
title: Cooperating Claude Code hooks on one event need a shared claim file
type: lesson
---

# Cooperating Claude Code hooks on one event need a shared claim file

Two Claude Code hooks registered on the same event (e.g. `simplify-gate` and `reusable-gate`, both `PostToolUse` on `Edit|Write|MultiEdit`) each receive **the same stdin payload** for one tool call, so both fire and the user gets two prompts for a single edit. Neither hook can see the other, so they need an out-of-band handshake: hash the raw stdin payload into an `eventHash` — both derive the same value, with no IPC and no ordering assumption — and let exactly one hook claim that event. The loser persists its accumulated counters **without spending a pass** and exits silently, so it fires on the next edit instead of being dropped.

> [!warning] The obvious implementation is wrong
> Hooks on the same matcher run **concurrently**. "Read the claim file, and if nobody owns this event, write my claim" is two operations: both hooks pass the check before either writes, both claim, both fire. This *looks* correct in every sequential test and fails in production. I shipped exactly this and then watched both gates fire repeatedly in the same session.

**Put the hash in the claim FILENAME, not its contents**, and take the claim with a single exclusive create:

- Node — `fs.writeFileSync(claimPath, body, { flag: 'wx' })`; catch `e.code === 'EEXIST'` to mean "the other hook owns this event".
- PowerShell — `[System.IO.File]::Open($p, [System.IO.FileMode]::CreateNew, …)`; it throws when the file exists.

The create *is* the lock, so there is no window. Per-event filenames buy a second thing for free: a leftover claim from a previous edit can never be mistaken for a claim on the current one, so there is no stale-claim arbitration and no hash comparison left to get wrong. The cost is accumulating tiny files in the state dir — bounded by the gates' own per-session pass cap.

Any other IO error on the create should **fire**, not defer — a full disk shouldn't silently disable the gate. Only a genuine "file already exists" means stand down.

The payload hash is what makes this work: it is derived from data both processes already have, identically, with no IPC and no ordering assumption.

```js
const eventHash = crypto.createHash("sha1").update(raw, "utf8").digest("hex");
const claim = loadJson(claimPath, null);
if (claim && claim.eventHash === eventHash && claim.gate !== "simplify") {
    fs.writeFileSync(statePath, JSON.stringify(state));  // keep counters, spend no pass
    process.exit(0);
}
fs.writeFileSync(claimPath, JSON.stringify({ eventHash, gate: "simplify" }));
```

Two failure modes make this fail *silently*, which is why they are worth naming:

- **Divergent state directories.** The claim file only works if both hooks resolve the *same* path. A Node hook using `~/.claude/plugin-state/<name>/` and a PowerShell hook using `~/.claude/hooks/state/` will each happily write a claim neither ever reads, and the double-fire continues with no error anywhere. A shared claim dir is a hard requirement of the protocol, not an implementation detail.
- **Port drift.** When the same hook ships as both a `.js` and a `.ps1` "behaviourally identical" pair, protocol changes get applied to whichever variant the author runs. Here the PowerShell port had the claim protocol and the *registered default* (the Node one) did not — so the shipped experience was the broken one.

Related: [[Installer skill templates drift from the live scripts they install]]

## Related

- [[Installer skill templates drift from the live scripts they install]]

%% ai-graph-start %%

**Related notes:**
- [[Cooperating PostToolUse hooks via a shared per-event SHA1 claim file]]
- [[PSScriptRoot-relative state breaks when a hook moves to a subfolder]]
- [[Claude Code hooks fire for any spawned claude process, not just interactive sessions]]
- [[Installer skill templates drift from the live scripts they install]]
- [[PowerShell pipe appends a newline to native-command stdin, shifting any hash]]

%% ai-graph-end %%