<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v0.9.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: A ${{ ... }} expression is interpolated directly inside a `run:` shell command string. In action.yml, the step 'Run bomdrift' uses `run: ${{ github.action_path }}/entrypoint.sh`. In comment-suppress/action.yml, the step 'Run bomdrift comment-suppress' uses the same pattern. Any ${{ ... }} expression inside a run: block is subject to YAML template substitution before the shell processes it, making it a script-injection risk regardless of which context it reads from.

Locations:

- `action.yml:271`
- `comment-suppress/action.yml:49`

### unpinned-uses (severity: high)

Multiple `uses:` references in action.yml and comment-suppress/action.yml are pinned to mutable tags or version strings instead of full 40-character commit SHAs. Failing references in action.yml: `actions/checkout@v4` (×2), `anchore/sbom-action/download-syft@v0`, `sigstore/cosign-installer@v3`, `github/codeql-action/upload-sarif@v3`. Failing reference in comment-suppress/action.yml: `sigstore/cosign-installer@v3`. These mutable refs can be silently redirected to different (potentially malicious) code by a supply-chain compromise of the referenced action.

Locations:

- `action.yml:250`
- `action.yml:257`
- `action.yml:264`
- `action.yml:268`
- `action.yml:310`
- `comment-suppress/action.yml:46`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all 6 unpinned-uses findings by resolving full commit SHAs for: actions/checkout@v4 (→11d5960a..., ×2), anchore/sbom-action/download-syft@v0 (→e22c3899...), sigstore/cosign-installer@v3 (→398d4b0e..., in both files), and github/codeql-action/upload-sarif@v3 (→9f759ee6...). Fixed both script-injection findings by moving ${{ github.action_path }} out of run: into the env: block as ACTION_PATH, then referencing it as $ACTION_PATH/entrypoint.sh in the shell command. The existing env: blocks were preserved; ACTION_PATH was added as the first entry to avoid duplicate YAML keys.

