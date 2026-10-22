<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift--comment-suppress/v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift--comment-suppress/v1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

A composite step uses an action by a mutable tag instead of a full 40-character commit SHA: `uses: sigstore/cosign-installer@v3`.

Locations:

- `action.yml:51`

### script-injection (severity: high)

Sub-rule (a): a GitHub Actions expression is placed directly inside a run: block: `run: ${{ github.action_path }}/entrypoint.sh`. Pass it through env instead and quote it, e.g. `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:54`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned sigstore/cosign-installer to 398d4b0eeef1380460a10c8013a76f728fb906ac (v3). Replaced run: ${{ github.action_path }}/entrypoint.sh with quoted "$GITHUB_ACTION_PATH/entrypoint.sh" (bash shell).

