<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift--comment-suppress/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift--comment-suppress/v0.9.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step uses `sigstore/cosign-installer@v3`, which is a mutable tag reference rather than a pinned 40-character SHA commit hash. This means the action could silently change if the tag is moved, enabling a supply-chain attack.

Locations:

- `action.yml:49`

### script-injection (severity: high)

Rule (a) violation: A `${{ }}` expression is interpolated directly inside a `run:` shell command string. The step at line 52 uses `run: ${{ github.action_path }}/entrypoint.sh`, which embeds the `github.action_path` context value directly into the shell command before the shell ever sees it. Any `${{ ... }}` expression in a `run:` block is a script-injection risk because the value is substituted by the YAML template engine before shell quoting applies. The safe alternative is to use the `$GITHUB_ACTION_PATH` environment variable instead: `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:52`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. Pinned sigstore/cosign-installer@v3 to full commit SHA 398d4b0eeef1380460a10c8013a76f728fb906ac (tag preserved as comment). 2. Replaced `${{ github.action_path }}/entrypoint.sh` in the run: block with `"$GITHUB_ACTION_PATH/entrypoint.sh"` to use the safe built-in environment variable instead of a template expression subject to script injection.

