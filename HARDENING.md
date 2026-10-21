<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/0.9.7-alpha

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/0.9.7-alpha** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Composite action steps reference actions by mutable tags rather than full commit SHAs: `uses: actions/checkout@v4` (twice), `uses: anchore/sbom-action/download-syft@v0`, `uses: sigstore/cosign-installer@v3`, `uses: github/codeql-action/upload-sarif@v3`.

Locations:

- `action.yml:295`
- `action.yml:304`
- `action.yml:317`
- `action.yml:321`

### unpinned-uses (severity: high)

The comment-suppress sub-action references an action by a mutable tag rather than a full commit SHA: `uses: sigstore/cosign-installer@v3`.

Locations:

- `comment-suppress/action.yml:48`

### script-injection (severity: high)

Sub-rule (a): a GitHub expression is placed directly inside a run: script: `run: ${{ github.action_path }}/entrypoint.sh`. Use the env var instead, e.g. `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:325`

### script-injection (severity: high)

Sub-rule (a): a GitHub expression is placed directly inside a run: script: `run: ${{ github.action_path }}/entrypoint.sh`. Use `"$GITHUB_ACTION_PATH/entrypoint.sh"` instead.

Locations:

- `comment-suppress/action.yml:51`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned actions/checkout (x2), anchore/sbom-action/download-syft, sigstore/cosign-installer (in both manifests) and github/codeql-action/upload-sarif to full commit SHAs, keeping the tag as a comment after each one. In action.yml and comment-suppress/action.yml, changed `run: ${{ github.action_path }}/entrypoint.sh` to `run: '"$GITHUB_ACTION_PATH/entrypoint.sh"'` (both steps use shell: bash). Running the edits in parallel damaged action.yml: the 'Run bomdrift' step header was lost and the SARIF step's name and if: lines were merged together. I repaired both and checked the file. Nothing was run to test the changes.

