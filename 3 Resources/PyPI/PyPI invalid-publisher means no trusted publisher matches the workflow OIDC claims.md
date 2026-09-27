---
title: "PyPI invalid-publisher means no trusted publisher matches the workflow OIDC claims"
created: 2026-09-19
type: lesson
status: seedling
source: "session 2026-09-19"
tags: [pypi, github-actions, oidc, trusted-publishing, ci-cd, gotcha]
---

# PyPI invalid-publisher means no trusted publisher matches the workflow OIDC claims

PyPI Trusted Publishing (OIDC) fails with `Trusted publishing exchange failure` → `invalid-publisher: valid token, but no corresponding publisher` when the GitHub OIDC token is valid but **no trusted publisher registered on PyPI matches the token's claims**. It is a **PyPI-side configuration gap, not a code/workflow bug**.

**Fix:** register a Trusted Publisher on PyPI whose fields match the run's OIDC claims exactly:
- **owner** (e.g. `LEO-CDP`)
- **repository name** (e.g. `leo-customer360`)
- **workflow filename** (e.g. `publish-customer360-dao.yml`)
- **environment name** — must be set if the job declares `environment:` (e.g. `pypi`); leaving it blank on PyPI keeps failing.

For a **first release** (project not yet on PyPI) use *Account → Publishing → Add a **pending** publisher*; PyPI creates the project and promotes the pending publisher to permanent on first successful upload. For an existing project use *project → Manage → Publishing*.

**Key nuance:** matching is on **owner + repo + workflow-filename + environment** — **not** the git branch/ref. So the job can run from any branch as long as that workflow file path is used. The claims panel PyPI prints on failure (`sub`, `repository`, `workflow_ref`, `environment`) is for debugging only — do not blindly copy it into config unless it already matches expectations.

Fallback if you can't administer the PyPI project: drop OIDC and pass `password: ${{ secrets.PYPI_API_TOKEN }}` (and remove `id-token: write`).

The error is often buried below a red herring — see [["Unable to find image locally" is normal Docker pre-pull output, not the failure]].

## Related

- [["Unable to find image locally" is normal Docker pre-pull output]]
- [[not the failure]]
