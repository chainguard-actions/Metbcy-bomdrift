<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v1** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml uses actions by tag rather than by full commit SHA: `uses: actions/checkout@v4` (twice), `uses: anchore/sbom-action/download-syft@v0`, `uses: sigstore/cosign-installer@v3`, `uses: github/codeql-action/upload-sarif@v3`.

Locations:

- `action.yml:305`
- `action.yml:314`
- `action.yml:326`
- `action.yml:330`
- `action.yml:389`

### unpinned-uses (severity: high)

comment-suppress/action.yml uses an action by tag rather than by full commit SHA: `uses: sigstore/cosign-installer@v3`.

Locations:

- `comment-suppress/action.yml:48`

### script-injection (severity: high)

Sub-rule (a): an expression is placed directly in a run: block. Offending line: `run: ${{ github.action_path }}/entrypoint.sh`. Fix: use the env var instead, as `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:334`

### script-injection (severity: high)

Sub-rule (a): an expression is placed directly in a run: block. Offending line: `run: ${{ github.action_path }}/entrypoint.sh`. Fix: use the env var instead, as `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `comment-suppress/action.yml:51`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned actions/checkout (x2), anchore/sbom-action/download-syft, sigstore/cosign-installer (both manifests), github/codeql-action/upload-sarif to full SHAs with tag comments. Replaced `run: ${{ github.action_path }}/entrypoint.sh` with `run: '"$GITHUB_ACTION_PATH/entrypoint.sh"'` (shell: bash) in action.yml and comment-suppress/action.yml.

