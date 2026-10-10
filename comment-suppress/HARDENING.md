<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift--comment-suppress/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift--comment-suppress/v0.9.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action uses `sigstore/cosign-installer@v3`, which is pinned to a mutable version tag rather than an immutable 40-character commit SHA. If the tag is moved (intentionally or via a supply-chain compromise), the action will silently execute different code. Pin to a full SHA, e.g. `sigstore/cosign-installer@3454791bac868b0e6b5f7e8b5e0b5e0b5e0b5e0b # v3`.

Locations:

- `action.yml:50`

### script-injection (severity: high)

Rule (a) violation: a `${{ ... }}` expression is interpolated directly inside a `run:` shell command string. The step `run: ${{ github.action_path }}/entrypoint.sh` embeds `${{ github.action_path }}` directly in the shell command before the shell ever sees it. Although `github.action_path` is not directly attacker-controlled, any `${{ ... }}` expression inside a `run:` block is a script-injection risk because the value is substituted into the shell command string by the Actions template engine before the shell parses it. The safe alternative is to use the pre-set environment variable `$GITHUB_ACTION_PATH` instead: `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:53`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed two findings in hardened/action/action.yml: (1) Pinned sigstore/cosign-installer@v3 to the immutable SHA 398d4b0eeef1380460a10c8013a76f728fb906ac, preserving the tag as a comment. (2) Replaced `${{ github.action_path }}/entrypoint.sh` in the run: block with `"$GITHUB_ACTION_PATH/entrypoint.sh"`, using the pre-set environment variable instead of a template expression to eliminate the script-injection risk.

