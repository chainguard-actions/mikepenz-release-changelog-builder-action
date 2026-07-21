<!-- markdownlint-disable -->

# Hardening Report: mikepenz--release-changelog-builder-action/v6.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mikepenz--release-changelog-builder-action/v6.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Commit rebuilt dist' step in the commit-dist job directly interpolates GitHub Actions expressions into a shell run: command. The line `git push https://x-access-token:${{ secrets.RENOVATE_TOKEN }}@github.com/${{ github.repository }}.git HEAD:${{ github.event.pull_request.head.ref }}` embeds ${{ github.repository }} and ${{ github.event.pull_request.head.ref }} directly into the shell string. These values flow through YAML template substitution before the shell processes them, allowing an attacker to inject shell metacharacters via a crafted repository name or branch ref. These should be passed via environment variables and referenced as quoted shell variables (e.g., "$REPO" and "$HEAD_REF").

Locations:

- `.github/workflows/ci.yml:81`

### missing-permissions (severity: medium)

The workflow file renovate.yml has no top-level `permissions:` key and the sole `renovate` job also has no `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal permissions block (e.g., `contents: read`) should be added at the job level.

Locations:

- `.github/workflows/renovate.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, missing-permissions

**Notes:**

1. Fixed script-injection in .github/workflows/ci.yml (line 81): Moved `${{ github.repository }}`, `${{ github.event.pull_request.head.ref }}`, and `${{ secrets.RENOVATE_TOKEN }}` out of the shell `run:` string and into the step's `env:` block as `REPO`, `HEAD_REF`, and `RENOVATE_TOKEN`. The `git push` command now references these as quoted shell variables `${REPO}`, `${HEAD_REF}`, and `${RENOVATE_TOKEN}`. 2. Fixed missing-permissions in .github/workflows/renovate.yml: Added `permissions: contents: read` at the job level for the `renovate` job, replacing the implicit broad default token permissions with the minimum required.

