<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift--comment-suppress/0.9.7-alpha

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift--comment-suppress/0.9.7-alpha** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: A ${{ ... }} expression is interpolated directly inside a `run:` shell command string. The line `run: ${{ github.action_path }}/entrypoint.sh` embeds `${{ github.action_path }}` directly into the shell command before the shell ever sees it. Per the check rules, any `${{ ... }}` expression inside a `run:` block is a script-injection finding. The safe alternative is to use the `$GITHUB_ACTION_PATH` environment variable instead: `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:50`

### unpinned-uses (severity: high)

The composite action step uses `sigstore/cosign-installer@v3`, which is pinned to a mutable version tag (`@v3`) rather than an immutable 40-character commit SHA. If the tag is moved (intentionally or via a supply-chain compromise), the action will silently execute different code. It should be pinned to a full SHA, e.g. `sigstore/cosign-installer@3454791b5f3e91534e9f3e4b6e3b6e3b6e3b6e3b # v3`.

Locations:

- `action.yml:47`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. Pinned sigstore/cosign-installer from mutable tag @v3 to full commit SHA @398d4b0eeef1380460a10c8013a76f728fb906ac # v3. 2. Replaced `${{ github.action_path }}/entrypoint.sh` in the run: block with `"$GITHUB_ACTION_PATH/entrypoint.sh"`, using the built-in GITHUB_ACTION_PATH environment variable to eliminate the script-injection risk.

