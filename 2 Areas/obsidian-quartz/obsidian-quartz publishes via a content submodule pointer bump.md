---
ai_hash: 2b34bbe412ce5acf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-31
entities:
- obsidian-quartz
- Quartz static-site generator
- content/ directory
- git submodule
- obsidian-vault repo
- C:\obsidian-vault
- PARA/Zettelkasten
- publish.sh
- GitHub Actions
- Deploy Quartz site to GitHub Pages workflow
- GitHub Pages
- vault remote
- automated vault-backup process
- git add content
- git submodule update --remote
- git merge-base
- liem18112000-axon/obsidian-quartz/actions
- Quartz superproject
- submodule pointer
- vault HEAD
- public Quartz site
- notes
- Publish flow
- vault edit
- local vs upstream
- commit edits
- reconcile with vault remote
- push to obsidian-vault repo
- commit (vault bump)
- push to obsidian-quartz
source: session 2026-07-31 quartz vault sync
status: seedling
tags:
- quartz
- obsidian
- git-submodule
- github-pages
- publishing
title: obsidian-quartz publishes via a content submodule pointer bump
type: concept
---

# obsidian-quartz publishes via a content submodule pointer bump

The published site at `obsidian-quartz` (Quartz static-site generator) does **not** contain the notes directly. Its `content/` directory is a **git submodule** pointing at the separate `obsidian-vault` repo. Two repos, two push targets.

> [!important] `content/` is a symlink to the live vault
> `c:/quartz/content` is a **symlink → `C:\obsidian-vault`** — the *same* vault used for PARA/Zettelkasten knowledge capture. So every note captured into `C:\obsidian-vault` becomes source for the **public** Quartz site. There is no private staging area; curate at publish time.

**Publish flow (`publish.sh`):**
1. In `content/` (the vault): commit any edits, reconcile with the vault remote, and push → `obsidian-vault` advances.
2. In the Quartz superproject: `git add content` to bump the recorded submodule pointer to the new vault HEAD, commit `(vault bump)`, and push → `obsidian-quartz` advances.
3. The push to `obsidian-quartz` triggers the GitHub Actions **"Deploy Quartz site to GitHub Pages"** workflow (~3 min) which builds and publishes.

**Implications:**
- A vault edit is not live until *both* pushes happen AND the pointer is bumped — pushing only the vault does nothing to the site.
- An **automated vault-backup process** independently commits + pushes the vault (commits titled `vault backup: <timestamp>`). So untracked notes can reach the vault remote *without* an explicit `publish.sh` run — but the **superproject pointer bump still must happen** for them to go live. In practice: capture a note → auto-backup pushes the vault → run `publish.sh` (its step 1 becomes a no-op; step 2 bumps + pushes) → deploy.
- Watch deploys at `https://github.com/liem18112000-axon/obsidian-quartz/actions`.
- Do the bump with `git add content`, not `git submodule update --remote` — see [[git submodule update --remote can clobber unpushed submodule HEAD]].
- Reconcile the vault safely with [[Classify local vs upstream with git merge-base to pick ff or rebase]].

## Related

- [[git submodule update --remote can clobber unpushed submodule HEAD]]
- [[Classify local vs upstream with git merge-base to pick ff or rebase]]

%% ai-graph-start %%

**Related notes:**
- [[Publish]]
- [[Classify local vs upstream with git merge-base to pick ff or rebase]]
- [[git submodule update --remote can clobber unpushed submodule HEAD]]
- [[My knowledge ecosystem Claude hooksskills - Obsidian vault - Quartz wiki + vault-graph (Vertex Graph RAG)]]
- [[Stage 6 — Advanced Automation]]

**Relations:**
- obsidian-quartz — *IS_A* — Quartz static-site generator
- obsidian-quartz — *PUBLISHES_VIA* — submodule pointer bump
- content/ directory — *IS_A* — git submodule
- git submodule — *POINTS_AT* — obsidian-vault repo
- content/ directory — *IS_SYMLINK_TO* — C:\obsidian-vault
- C:\obsidian-vault — *USED_FOR* — PARA/Zettelkasten
- publish.sh — *IMPLEMENTS* — Publish flow
- Publish flow — *HAS_STEP* — commit edits
- Publish flow — *HAS_STEP* — reconcile with vault remote
- Publish flow — *HAS_STEP* — push to obsidian-vault repo
- Publish flow — *HAS_STEP* — git add content
- Publish flow — *HAS_STEP* — commit (vault bump)
- Publish flow — *HAS_STEP* — push to obsidian-quartz
- push to obsidian-quartz — *TRIGGERS* — GitHub Actions
- GitHub Actions — *EXECUTES* — Deploy Quartz site to GitHub Pages workflow
- Deploy Quartz site to GitHub Pages workflow — *PUBLISHES_TO* — GitHub Pages
- vault edit — *REQUIRES* — push to obsidian-quartz
- vault edit — *REQUIRES* — submodule pointer bump
- automated vault-backup process — *COMMITS_AND_PUSHES* — C:\obsidian-vault
- automated vault-backup process — *PUSHES_TO* — vault remote
- git add content — *BUMPS* — submodule pointer
- git submodule update --remote — *CAN* — clobber unpushed submodule HEAD
- git merge-base — *HELPS* — Classify local vs upstream
- liem18112000-axon/obsidian-quartz/actions — *SHOWS* — deploys
- content/ directory — *IS_PART_OF* — Quartz superproject
- submodule pointer — *POINTS_TO* — vault HEAD
- C:\obsidian-vault — *IS_SOURCE_FOR* — public Quartz site
- public Quartz site — *IS* — obsidian-quartz
- obsidian-vault repo — *IS_REMOTE_FOR* — vault remote
- notes — *ARE_IN* — C:\obsidian-vault
- obsidian-quartz — *USES* — content/ directory
- obsidian-quartz — *USES* — obsidian-vault repo
- content/ directory — *DOES_NOT_CONTAIN* — notes directly
- obsidian-vault repo — *ADVANCES_WITH* — push to obsidian-vault repo
- obsidian-quartz — *ADVANCES_WITH* — push to obsidian-quartz
- C:\obsidian-vault — *IS_A* — vault
- obsidian-vault repo — *IS_A* — vault
- vault remote — *IS_A* — remote
- git add content — *IS_A* — command
- git submodule update --remote — *IS_A* — command
- git merge-base — *IS_A* — command
- commit edits — *OCCURS_IN* — content/ directory
- git add content — *OCCURS_IN* — Quartz superproject
- commit (vault bump) — *OCCURS_IN* — Quartz superproject
- push to obsidian-vault repo — *TARGETS* — obsidian-vault repo
- push to obsidian-quartz — *TARGETS* — obsidian-quartz

%% ai-graph-end %%