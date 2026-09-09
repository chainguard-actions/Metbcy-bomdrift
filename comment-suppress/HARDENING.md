<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift--comment-suppress/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift--comment-suppress/v0.9.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step uses `sigstore/cosign-installer@v3`, which is pinned to a mutable version tag (`@v3`) rather than an immutable 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit at any time, making this a supply-chain risk.

Locations:

- `action.yml:49`

### script-injection (severity: high)

Sub-rule (a): The `run:` block directly interpolates the GitHub Actions expression `${{ github.action_path }}` inside the shell command string: `run: ${{ github.action_path }}/entrypoint.sh`. Any `${{ ... }}` expression embedded directly in a `run:` block is subject to template substitution before the shell sees it, bypassing shell quoting. This should be replaced with the pre-set environment variable `$GITHUB_ACTION_PATH` instead.

Locations:

- `action.yml:52`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. Pinned sigstore/cosign-installer@v3 to full commit SHA 398d4b0eeef1380460a10c8013a76f728fb906ac (tag preserved as comment). 2. Replaced ${{ github.action_path }} in the run: block with the pre-set environment variable $GITHUB_ACTION_PATH to eliminate template injection risk.

