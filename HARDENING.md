<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Metbcy--bomdrift/v0.9.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in action.yml and comment-suppress/action.yml are pinned to mutable tags instead of full 40-character commit SHAs. Mutable tags can be moved by the upstream maintainer to point to different (potentially malicious) commits at any time, enabling supply-chain attacks.

Failing references in action.yml:
- `uses: actions/checkout@v4`  (appears twice)
- `uses: anchore/sbom-action/download-syft@v0`
- `uses: sigstore/cosign-installer@v3`
- `uses: github/codeql-action/upload-sarif@v3`

Failing references in comment-suppress/action.yml:
- `uses: sigstore/cosign-installer@v3`

All should be pinned to a full SHA, e.g. `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `action.yml:289`
- `action.yml:296`
- `action.yml:302`
- `action.yml:307`
- `action.yml:388`
- `comment-suppress/action.yml:47`

### script-injection (severity: high)

Sub-rule (a): A `${{ github.action_path }}` expression is interpolated directly inside a `run:` shell command string in both action.yml and comment-suppress/action.yml. Per the script-injection check, ANY `${{ ... }}` expression inside a `run:` block is a finding — the value flows through YAML template substitution before the shell ever sees it, bypassing shell quoting. The offending lines are:

  action.yml:          `run: ${{ github.action_path }}/entrypoint.sh`
  comment-suppress/action.yml: `run: ${{ github.action_path }}/entrypoint.sh`

Fix: use the `$GITHUB_ACTION_PATH` environment variable instead, which is already set by the runner and does not require template interpolation:
  `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`

Locations:

- `action.yml:312`
- `comment-suppress/action.yml:50`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all unpinned `uses:` references in action.yml and comment-suppress/action.yml by pinning to full commit SHAs: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5, anchore/sbom-action/download-syft@v0 → @e22c389904149dbc22b58101806040fa8d37a610, sigstore/cosign-installer@v3 → @398d4b0eeef1380460a10c8013a76f728fb906ac, github/codeql-action/upload-sarif@v3 → @dd903d2e4f5405488e5ef1422510ee31c8b32357. Fixed script-injection in both action.yml and comment-suppress/action.yml by replacing `run: ${{ github.action_path }}/entrypoint.sh` with `run: "$GITHUB_ACTION_PATH/entrypoint.sh"` to use the runner-provided environment variable instead of template interpolation.

