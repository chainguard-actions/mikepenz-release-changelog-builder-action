<!-- markdownlint-disable -->

# Hardening Report: mikepenz--release-changelog-builder-action/v6.2.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mikepenz--release-changelog-builder-action/v6.2.3** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The 'Commit rebuilt dist' step in the commit-dist job directly interpolates GitHub Actions expressions inside a run: shell command. The line `git push https://x-access-token:${{ secrets.RENOVATE_TOKEN }}@github.com/${{ github.repository }}.git HEAD:${{ github.event.pull_request.head.ref }}` embeds ${{ github.repository }} and ${{ github.event.pull_request.head.ref }} directly into the shell string. The value of github.event.pull_request.head.ref is attacker-controllable (a PR author can craft a branch name containing shell metacharacters), enabling command injection. These values should be passed via env: variables and referenced as quoted shell variables (e.g., "$REPO" and "$HEAD_REF") instead of being interpolated directly.

Locations:

- `.github/workflows/ci.yml:81`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the 'Commit rebuilt dist' step of the commit-dist job in .github/workflows/ci.yml. Moved ${{ secrets.RENOVATE_TOKEN }}, ${{ github.repository }}, and ${{ github.event.pull_request.head.ref }} out of the run: shell string and into the step's env: block as RENOVATE_TOKEN, REPO, and HEAD_REF. Updated the git push command to reference these as quoted shell variables (${RENOVATE_TOKEN}, ${REPO}, ${HEAD_REF}), preventing attacker-controlled branch names from being interpreted as shell commands.

