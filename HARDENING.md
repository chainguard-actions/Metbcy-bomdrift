<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v0.9.9** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Composite action steps point to mutable tags instead of 40-char commit SHAs: 'uses: actions/checkout@v4' (twice), 'uses: anchore/sbom-action/download-syft@v0', 'uses: sigstore/cosign-installer@v3', 'uses: github/codeql-action/upload-sarif@v3'.

Locations:

- `action.yml:290`
- `action.yml:299`
- `action.yml:311`
- `action.yml:315`
- `action.yml:384`

### unpinned-uses (severity: high)

Composite action step points to a mutable tag instead of a commit SHA: 'uses: sigstore/cosign-installer@v3'.

Locations:

- `comment-suppress/action.yml:50`

### script-injection (severity: high)

Sub-rule (a): a GitHub expression is placed directly in a run: block: 'run: ${{ github.action_path }}/entrypoint.sh' in the 'Run bomdrift' step. Pass it through env (e.g. use the runner-provided "$GITHUB_ACTION_PATH/entrypoint.sh") instead of template substitution.

Locations:

- `action.yml:319`

### script-injection (severity: high)

Sub-rule (a): a GitHub expression is placed directly in a run: block: 'run: ${{ github.action_path }}/entrypoint.sh' in the 'Run bomdrift comment-suppress' step. Use "$GITHUB_ACTION_PATH/entrypoint.sh" instead.

Locations:

- `comment-suppress/action.yml:53`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned actions/checkout (x2), anchore/sbom-action/download-syft, sigstore/cosign-installer (action.yml and comment-suppress/action.yml), and github/codeql-action/upload-sarif to full commit SHAs with tag comments. Replaced 'run: ${{ github.action_path }}/entrypoint.sh' with '"$GITHUB_ACTION_PATH/entrypoint.sh"' in both bash steps.

