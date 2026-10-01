<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/0.9.7-alpha

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/0.9.7-alpha** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A `github.*` context expression is interpolated directly inside a `run:` shell command string. In the 'Run bomdrift' step, `run: ${{ github.action_path }}/entrypoint.sh` embeds `${{ github.action_path }}` directly in the shell command. Although `github.action_path` is not attacker-controlled in the same way as `github.head_ref`, any `${{ ... }}` expression inside a `run:` block is a script-injection finding per the check rules — the value flows through YAML template substitution before the shell ever sees it. The same pattern appears in comment-suppress/action.yml. The fix is to use the `$GITHUB_ACTION_PATH` environment variable instead: `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:285`
- `comment-suppress/action.yml:50`

### unpinned-uses (severity: high)

Multiple `uses:` references in action.yml and comment-suppress/action.yml are pinned to mutable tags rather than immutable 40-character commit SHAs, making the action vulnerable to supply-chain attacks if any of those upstream actions are compromised or their tags are moved. Failing references in action.yml: `actions/checkout@v4` (two occurrences), `anchore/sbom-action/download-syft@v0`, `sigstore/cosign-installer@v3`, `github/codeql-action/upload-sarif@v3`. Failing reference in comment-suppress/action.yml: `sigstore/cosign-installer@v3`. All should be replaced with their full 40-hex-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `action.yml:263`
- `action.yml:270`
- `action.yml:278`
- `action.yml:282`
- `action.yml:338`
- `comment-suppress/action.yml:47`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script-injection in both action.yml and comment-suppress/action.yml by replacing `${{ github.action_path }}/entrypoint.sh` with `"$GITHUB_ACTION_PATH/entrypoint.sh"` (using the built-in env var instead of template expression). Fixed all 6 unpinned-uses references by pinning to full 40-char commit SHAs: actions/checkout@v4 → 11d5960a326750d5838078e36cf38b85af677262 (×2), anchore/sbom-action/download-syft@v0 → e22c389904149dbc22b58101806040fa8d37a610, sigstore/cosign-installer@v3 → 398d4b0eeef1380460a10c8013a76f728fb906ac (×2, in both files), github/codeql-action/upload-sarif@v3 → 1190a975f95ce23525efb6a3fc21ea29567c1b52. All original tags preserved as inline comments.

