<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/0.9.7-alpha

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/0.9.7-alpha** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in action.yml and comment-suppress/action.yml are pinned to mutable tag refs instead of immutable 40-character commit SHAs, making the action vulnerable to supply-chain attacks if those tags are moved. Failing references:
- action.yml: `uses: actions/checkout@v4` (×2)
- action.yml: `uses: anchore/sbom-action/download-syft@v0`
- action.yml: `uses: sigstore/cosign-installer@v3`
- action.yml: `uses: github/codeql-action/upload-sarif@v3`
- comment-suppress/action.yml: `uses: sigstore/cosign-installer@v3`

Locations:

- `action.yml:244`
- `action.yml:252`
- `action.yml:260`
- `action.yml:264`
- `action.yml:310`
- `comment-suppress/action.yml:47`

### script-injection (severity: high)

Sub-rule (a): A `${{ ... }}` expression is interpolated directly inside a `run:` shell command string. Both composite action steps use `run: ${{ github.action_path }}/entrypoint.sh`. Although `github.action_path` is GitHub-controlled, any `${{ ... }}` expression directly in a `run:` block undergoes YAML template substitution before the shell processes it, making it a script-injection risk. The safe alternative is to reference the path via the `$GITHUB_ACTION_PATH` environment variable instead.

Locations:

- `action.yml:267`
- `comment-suppress/action.yml:50`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all 6 unpinned `uses:` references by resolving them to full 40-character commit SHAs via lookup_action_sha (keeping tag comments for readability). Fixed both script-injection instances by replacing `${{ github.action_path }}/entrypoint.sh` with `$GITHUB_ACTION_PATH/entrypoint.sh` in both action.yml and comment-suppress/action.yml.

