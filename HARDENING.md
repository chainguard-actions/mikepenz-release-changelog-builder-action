<!-- markdownlint-disable -->

# Hardening Report: mikepenz--release-changelog-builder-action/v6.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mikepenz--release-changelog-builder-action/v6.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### broad-permissions (severity: medium)

scorecard.yml has a top-level `permissions: read-all` which grants overly broad read access to all scopes. This should be replaced with specific minimal permissions scoped to what the workflow actually needs (e.g., `security-events: write` and `id-token: write` as already defined at the job level).

Locations:

- `.github/workflows/scorecard.yml:9`

## Iteration Notes

### Iteration 1

**Fixes applied:** broad-permissions

**Notes:**

Replaced `permissions: read-all` at the top level of `.github/workflows/scorecard.yml` with `permissions: contents: read`. This is the minimal permission needed at the workflow level (for the checkout step). The job-level permissions (`security-events: write` and `id-token: write`) were already correctly defined and remain unchanged.

