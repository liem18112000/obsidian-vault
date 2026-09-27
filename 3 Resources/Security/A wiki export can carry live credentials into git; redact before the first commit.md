---
title: "A wiki export can carry live credentials into git; redact before the first commit"
created: 2026-09-27
type: lesson
status: seedling
source: "session 2026-09-27 (Confluence import push blocked by GitHub secret scanning)"
tags: [security, secrets, git, migration, push-protection, gcp, gotcha]
---

# A wiki export can carry live credentials into git; redact before the first commit

Importing 167 Confluence pages into a git-backed vault tripped **GitHub push protection**: two pages had complete **GCP service-account JSON blobs**, private keys included, pasted inline as Kubernetes config examples.

The mechanism is worth naming. A wiki is a **low-friction, high-trust** surface — people paste a working config to help a colleague, and it stays there for years because nothing scans it. Exporting that wiki into a **version-controlled** repository silently changes the exposure: what was behind SSO becomes a commit, and once pushed, git keeps it forever even if a later commit removes it.

So the rule is about **ordering**: scan and redact **before** the first commit, not before the first push. I had already committed when the push was blocked, which meant rewriting history (`git reset --soft <upstream>` → redact → recommit) rather than just editing a file. Unpushed commits make that cheap; one push makes it expensive.

What to scan an export for, beyond the obvious:

- `-----BEGIN (RSA |EC )?PRIVATE KEY-----`
- `"type": "service_account"` and `"private_key"` (GCP)
- `AKIA[0-9A-Z]{16}` (AWS), `ghp_` / `github_pat_` (GitHub), `xox[baprs]-` (Slack)
- `.env` fragments and connection strings with inline passwords

Two judgement calls that mattered:

- **Redact the value, keep the shape.** Replacing the key with `<REDACTED>` while leaving `project_id` and `client_email` preserves the page's documentation value. Deleting the whole block would have destroyed why the page existed.
- **Distinguish a real key from a placeholder.** A third hit was `local.private.key=-----BEGIN PRIVATE KEY-----\n...` — a literal `...`, i.e. documentation of the property shape. Redacting it would have been noise. Match on key *material*, not on the marker alone.

And the part a git fix does not solve: **the credentials are still live in the wiki.** Removing them from a pre-push commit prevents a new exposure; it does nothing about the original. Rotation is the actual remediation.

## Related

- [[Verify a migration by reference parity with the source, not internal consistency]]

## Related

- [[Verify a migration by reference parity with the source]]
- [[not internal consistency]]
