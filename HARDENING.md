<!-- markdownlint-disable -->

# Hardening Report: mikepenz--release-changelog-builder-action/v6.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mikepenz--release-changelog-builder-action/v6.2.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The 'Commit rebuilt dist' step in the commit-dist job directly interpolates GitHub Actions expressions inside a `run:` shell command string. The line `git push https://x-access-token:${{ secrets.RENOVATE_TOKEN }}@github.com/${{ github.repository }}.git HEAD:${{ github.event.pull_request.head.ref }}` embeds `${{ secrets.RENOVATE_TOKEN }}`, `${{ github.repository }}`, and `${{ github.event.pull_request.head.ref }}` directly in the shell command. In particular, `github.event.pull_request.head.ref` is attacker-controlled and could contain shell metacharacters or newlines that alter command execution. All `${{ ... }}` expressions must be moved to `env:` variables and then referenced as quoted shell variables (e.g., `"$VAR"`) to prevent injection.

Locations:

- `.github/workflows/ci.yml:81`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the 'Commit rebuilt dist' step of the commit-dist job in .github/workflows/ci.yml. Moved ${{ secrets.RENOVATE_TOKEN }}, ${{ github.repository }}, and ${{ github.event.pull_request.head.ref }} out of the run: shell command and into an env: block as RENOVATE_TOKEN, REPOSITORY, and HEAD_REF respectively. The git push command now references these as quoted shell variables (${RENOVATE_TOKEN}, ${REPOSITORY}, ${HEAD_REF}), preventing the attacker-controlled head.ref value from injecting shell metacharacters.

