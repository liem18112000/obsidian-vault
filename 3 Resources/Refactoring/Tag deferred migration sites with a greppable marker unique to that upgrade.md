---
title: "Tag deferred migration sites with a greppable marker unique to that upgrade"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Upgrade Ivy Known issues (X4)"
tags: [migration, upgrade, refactoring, technical-debt, deprecation, confluence-distilled]
---

# Tag deferred migration sites with a greppable marker unique to that upgrade

During a framework or platform upgrade you constantly hit code that needs changing but not *right now* — a deprecated call, a workaround, a thing that compiles but warns. Leave a **greppable marker** at each site rather than keeping the list somewhere else:

```java
// TODO upgrade ivy
```

Then the cleanup backlog is generated from the code:

```bash
grep -rn "TODO upgrade ivy" --include=*.java .
```

The upgrade notes for the project literally open with this instruction — *"check all java code where has this hashtag"* — which is the tell that it worked: the marker, not a wiki page, is the source of truth.

**Why this beats a list in a ticket or a document:**

- **It cannot drift.** A hand-maintained list goes stale the moment someone fixes an item and forgets to tick it. The grep is always accurate because the marker lives with the code it describes.
- **It survives merges and refactors.** Move the method, the marker moves with it. A line number in a ticket does not.
- **Reviewers see it in the diff.** A marker in the changed hunk prompts "is this still needed?" at exactly the right moment.
- **It is countable.** `grep -c` gives a burn-down for the upgrade with no bookkeeping.

Make the tag **specific to the migration** — `TODO upgrade ivy`, not a bare `TODO`. A generic marker drowns in the thousands of unrelated TODOs already in the codebase, which is precisely why generic TODOs get ignored.

> [!tip] Deprecation warnings often hand you the replacement verbatim
> The warnings from this upgrade name the exact substitute:
> ```
> Method getCustomVarCharField1 of class ICase is deprecated.
>   Instead use customFields().stringField("CustomVarCharField1").getOrNull()
> Method setAdditionalProperty(String, String) is deprecated.
>   Instead use customFields().textField(String).set(String)
> ```
> When a platform writes warnings this well, the migration is mechanical — collect the warnings from one full build or run, and they *are* the work list. Pair that with the marker tag: the warning tells you what to write, the marker tells you where you deferred it.

> [!warning] A marker with no end date is just a comment
> Markers only work if the upgrade has a point where `grep` must return zero. Without that gate they accumulate across three successive upgrades and become archaeology. Make "no markers remain" part of the upgrade's definition of done.

Source: [[Upgrade Ivy - Known issues]] (X4, Confluence).
