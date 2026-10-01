<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift--comment-suppress/0.9.7-alpha

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift--comment-suppress/0.9.7-alpha** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step uses `sigstore/cosign-installer@v3`, which is a mutable tag reference rather than a pinned 40-character SHA commit hash. A tag can be moved to point to a different (potentially malicious) commit at any time, making this a supply-chain risk.

Locations:

- `action.yml:50`

### script-injection (severity: high)

Rule (a) violation: A `${{ }}` expression is interpolated directly inside a `run:` shell command string. The line `run: ${{ github.action_path }}/entrypoint.sh` embeds `${{ github.action_path }}` directly into the shell command before the shell ever sees it. Any `${{ ... }}` expression inside a `run:` block — including `github.action_path` — is subject to YAML template substitution prior to shell execution, making it a script-injection risk. The safe alternative is to use the `$GITHUB_ACTION_PATH` environment variable instead: `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:53`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed two findings in hardened/action/action.yml: (1) Pinned sigstore/cosign-installer from mutable tag @v3 to full commit SHA @398d4b0eeef1380460a10c8013a76f728fb906ac with a # v3 comment for readability. (2) Replaced the script-injection risk `run: ${{ github.action_path }}/entrypoint.sh` with the safe environment variable form `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`, which avoids YAML template substitution before shell execution.

