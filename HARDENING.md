<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/0.9.7-alpha

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Metbcy--bomdrift/0.9.7-alpha** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A `${{ ... }}` expression is interpolated directly inside a `run:` shell command string. Both action.yml and comment-suppress/action.yml use `run: ${{ github.action_path }}/entrypoint.sh`, embedding the `github.action_path` context value directly into the shell command before the shell ever sees it. Per the check rules, any `${{ ... }}` expression inside a `run:` block is a script-injection finding regardless of which context it reads from. Fix: use the `$GITHUB_ACTION_PATH` environment variable instead (e.g., `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`).

Locations:

- `action.yml:275`
- `comment-suppress/action.yml:55`

### unpinned-uses (severity: high)

Multiple `uses:` references are pinned to mutable tags or version strings rather than immutable 40-character commit SHAs, making the action vulnerable to supply-chain attacks if those tags are moved or compromised. Failing references in action.yml: `actions/checkout@v4` (×2), `anchore/sbom-action/download-syft@v0`, `sigstore/cosign-installer@v3`, `github/codeql-action/upload-sarif@v3`. Failing reference in comment-suppress/action.yml: `sigstore/cosign-installer@v3`. All should be replaced with full SHA digests (e.g., `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

Locations:

- `action.yml:252`
- `action.yml:260`
- `action.yml:268`
- `action.yml:272`
- `action.yml:330`
- `comment-suppress/action.yml:52`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script-injection in both action.yml and comment-suppress/action.yml by replacing `run: ${{ github.action_path }}/entrypoint.sh` with `run: "$GITHUB_ACTION_PATH/entrypoint.sh"` (using the built-in env var instead of expression interpolation). Fixed all unpinned-uses by pinning to full commit SHAs: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 (×2), anchore/sbom-action/download-syft@v0 → @e22c389904149dbc22b58101806040fa8d37a610, sigstore/cosign-installer@v3 → @398d4b0eeef1380460a10c8013a76f728fb906ac (×2, in both files), github/codeql-action/upload-sarif@v3 → @dd903d2e4f5405488e5ef1422510ee31c8b32357. All tag names preserved as comments.

