---
title: "GitHub fetch credentials PAT Bearer anonymous public Enterprise api-v3 raw Contents API"
created: 2026-09-23
type: term
status: seedling
source: "session 2026-09-23 add-github-support"
tags: [github, credentials, pat, rest-api, enterprise]
---

# GitHub fetch credentials PAT Bearer anonymous public Enterprise api-v3 raw Contents API

What credentials does GitHub need to fetch/explore a repo the way we do with Bitbucket, and how do public vs Enterprise differ.

**Credential = a Personal Access Token (PAT).** Two kinds work:
- **Fine-grained PAT** — Repository permission **Contents: Read-only** on the target repos (least privilege; preferred).
- **Classic PAT** — scope `repo` (needed for private) or `public_repo` (public only).

Sent as an HTTP header: `Authorization: Bearer <token>` (GitHub also accepts `token <token>`). GitHub REST also still accepts Basic auth with the token as the password, but Bearer is the clean modern form and needs no username.

**Public GitHub** — API base `https://api.github.com`. **Public repos work with NO token** (anonymous), just rate-limited to **60 requests/hour**; a token lifts it to 5000/h and unlocks private repos.

**GitHub Enterprise Server (self-hosted)** — same PAT model, but the REST API lives at `https://<host>/api/v3` (GraphQL at `/api/graphql`). Token must be created on that instance. A single deployment targets EITHER public OR one Enterprise host, chosen by env `GITHUB_API_BASE` (defaults to public).

**Fetching raw file content:** use the **Contents API** — `GET {base}/repos/{owner}/{repo}/contents/{path}?ref={ref}` with header `Accept: application/vnd.github.raw`. That returns the file body directly (without the raw media type you get JSON with base64). Uniform across public + Enterprise (unlike raw.githubusercontent.com). **Ceiling:** the raw media type serves files up to ~100MB; larger blobs need the Git Blobs API / Git LFS.

Also recommended: send `X-GitHub-Api-Version: 2022-11-28` for stability, esp. on Enterprise.

Contrast with Bitbucket Cloud, which uses `(username, app_password)` **Basic** auth and `api.bitbucket.org/2.0/.../src/{ref}/{path}` for raw content. See [[KGA crawler fetches repo source files via client mixin plus NodeFetcher registered by kind]].

## Related

- [[KGA crawler fetches repo source files via client mixin plus NodeFetcher registered by kind]]
