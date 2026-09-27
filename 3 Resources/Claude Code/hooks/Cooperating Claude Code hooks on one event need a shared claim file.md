---
title: "Cooperating Claude Code hooks on one event need a shared claim file"
created: 2026-09-27
type: lesson
status: seedling
source: "luz-hooks-plugin simplify-gate PR, 2026-09-27"
tags: [claude-code, hooks, concurrency, gotcha]
---

# Cooperating Claude Code hooks on one event need a shared claim file

Two Claude Code hooks registered on the same event (e.g. `simplify-gate` and `reusable-gate`, both `PostToolUse` on `Edit|Write|MultiEdit`) each receive **the same stdin payload** for one tool call, so both fire and the user gets two prompts for a single edit. Neither hook can see the other, so they need an out-of-band handshake: hash the raw stdin payload into an `eventHash`, and have whichever hook runs first write `{eventHash, gate}` to a per-session claim file. The loser reads the claim, sees the same hash owned by a different `gate`, persists its accumulated counters **without spending a pass**, and exits silently — so it fires on the next edit instead of being dropped.

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
