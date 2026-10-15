<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v0.9.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in action.yml and comment-suppress/action.yml are pinned to mutable version tags instead of immutable 40-character SHA digests. This exposes the action to supply-chain attacks if any of the referenced actions are compromised or their tags are moved. Failing references: `actions/checkout@v4` (×2), `anchore/sbom-action/download-syft@v0`, `sigstore/cosign-installer@v3`, `github/codeql-action/upload-sarif@v3` in action.yml; `sigstore/cosign-installer@v3` in comment-suppress/action.yml.

Locations:

- `action.yml:244`
- `action.yml:251`
- `action.yml:258`
- `action.yml:262`
- `action.yml:310`
- `comment-suppress/action.yml:47`

### script-injection (severity: high)

Sub-rule (a) violation: `${{ github.action_path }}` is interpolated directly inside a `run:` shell command string in both action.yml and comment-suppress/action.yml. The offending lines are `run: ${{ github.action_path }}/entrypoint.sh`. Although `github.action_path` is not attacker-controlled in the same way as `github.head_ref`, any `${{ ... }}` expression directly inside a `run:` block undergoes YAML template substitution before the shell ever sees it, making it a script-injection finding per the check rules. The safe alternative is to use the `$GITHUB_ACTION_PATH` environment variable instead.

Locations:

- `action.yml:265`
- `comment-suppress/action.yml:50`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all 6 unpinned `uses:` references by pinning to full 40-character SHA digests with original tags preserved as comments: actions/checkout@v4 (×2) → SHA 11d5960a..., anchore/sbom-action/download-syft@v0 → SHA e22c3899..., sigstore/cosign-installer@v3 (×2, in action.yml and comment-suppress/action.yml) → SHA 398d4b0e..., github/codeql-action/upload-sarif@v3 → SHA ce5e7eea.... Fixed both script-injection occurrences by replacing `${{ github.action_path }}/entrypoint.sh` with `$GITHUB_ACTION_PATH/entrypoint.sh` in action.yml and comment-suppress/action.yml.

