<!-- markdownlint-disable -->

# Hardening Report: mikepenz--release-changelog-builder-action/v6.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mikepenz--release-changelog-builder-action/v6.2.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): In the `commit-dist` job's "Commit rebuilt dist" step, GitHub Actions expressions are interpolated directly inside a `run:` shell command. Specifically, `${{ github.repository }}` and `${{ github.event.pull_request.head.ref }}` are embedded directly in the `git push` URL string. The `head.ref` value is attacker-controlled (a pull request contributor can set the branch name to contain shell metacharacters), enabling command injection. The offending line is:

```
git push https://x-access-token:${{ secrets.RENOVATE_TOKEN }}@github.com/${{ github.repository }}.git HEAD:${{ github.event.pull_request.head.ref }}
```

These values should be moved to `env:` variables and the env vars should be double-quoted in the shell command.

Locations:

- `.github/workflows/ci.yml:81`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the `commit-dist` job's "Commit rebuilt dist" step in `.github/workflows/ci.yml`. Moved `${{ secrets.RENOVATE_TOKEN }}`, `${{ github.repository }}`, and `${{ github.event.pull_request.head.ref }}` out of the `run:` shell string into an `env:` block as `RENOVATE_TOKEN`, `REPOSITORY`, and `HEAD_REF` respectively. The `git push` command now uses double-quoted environment variable references (`"${RENOVATE_TOKEN}"`, `"${REPOSITORY}"`, `"${HEAD_REF}"`) to prevent shell injection from attacker-controlled values like the PR branch name.

