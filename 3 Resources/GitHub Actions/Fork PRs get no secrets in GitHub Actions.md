---
title: "Fork PRs get no secrets in GitHub Actions"
created: 2026-09-14
type: gotcha
status: seedling
source: "leo-customer360 ci.yml, PR #68, session 2026-09-14"
tags: [github-actions, ci, secrets, gotcha, fork-pr]
---

# Fork PRs get no secrets in GitHub Actions

A workflow triggered by \`pull_request\` from a **fork** (cross-repository PR) does **not** receive repository secrets — GitHub withholds them so untrusted fork code cannot exfiltrate them. Any \`${{ secrets.X }}\` reference silently resolves to an empty string in that run.

## Symptom
A step whose *required* input is backed by a secret hard-fails because the value is empty. Concrete case: \`dawidd6/action-send-mail@v4\` errors **"Input required and not supplied: from"** even though \`BREVO_SENDER_EMAIL\` is correctly set in repo Actions secrets — because the PR was from a fork. Same run also showed the E2E job seeing empty \`KEYCLOAK_CLIENT_SECRET\`, confirming it is run-wide, not a per-secret typo.

## Diagnose
\`\`\`bash
gh pr view <N> --repo <owner>/<repo> --json isCrossRepository,headRepository
# isCrossRepository: true  => fork PR => no secrets
\`\`\`

## Fix (guard-and-skip)
Add a prior step that reads the secret via \`env:\` and sets an output flag, then gate the real step on it:
\`\`\`yaml
- id: mailcfg
  if: always()
  env: { SENDER: "${{ secrets.BREVO_SENDER_EMAIL }}" }
  run: |
    if [ -n "$SENDER" ]; then echo "ok=true" >> "$GITHUB_OUTPUT";
    else echo "::warning::secrets absent; skipping"; echo "ok=false" >> "$GITHUB_OUTPUT"; fi
- if: ${{ always() && steps.mailcfg.outputs.ok == 'true' }}
  uses: dawidd6/action-send-mail@v4
\`\`\`
This mirrors the repo idiom (the E2E job already guards its Keycloak secrets the same way). Secrets present on repo-internal pushes/PRs -> step runs; absent on fork PRs -> step skips cleanly.

## The alternative and why not
\`pull_request_target\` *does* expose secrets to fork PRs, but it runs in the base-repo context with write perms — untrusted fork code with your secrets is a real security risk. Guard-and-skip is the safe default.

Real case: leo-customer360 \`.github/workflows/ci.yml\` notify job, PR #68 (2026-09-14).

## Related

- [[GitHub Actions]]
