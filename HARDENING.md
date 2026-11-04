<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v1** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml has composite steps that reference actions by mutable tag instead of a 40-char commit SHA: `uses: actions/checkout@v4` (line 299), `uses: actions/checkout@v4` (line 308), `uses: anchore/sbom-action/download-syft@v0` (line 321), `uses: sigstore/cosign-installer@v3` (line 325), `uses: github/codeql-action/upload-sarif@v3` (line 384).

Locations:

- `action.yml:299`
- `action.yml:308`
- `action.yml:321`
- `action.yml:325`
- `action.yml:384`

### unpinned-uses (severity: high)

comment-suppress/action.yml references an action by mutable tag: `uses: sigstore/cosign-installer@v3` (line 50).

Locations:

- `comment-suppress/action.yml:50`

### script-injection (severity: high)

Sub-rule (a): an expression is put straight into the run: command in the 'Run bomdrift' step: `run: ${{ github.action_path }}/entrypoint.sh`. Use the $GITHUB_ACTION_PATH env var in double quotes instead, e.g. `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:329`

### script-injection (severity: high)

Sub-rule (a): an expression is put straight into the run: command in the 'Run bomdrift comment-suppress' step: `run: ${{ github.action_path }}/entrypoint.sh`. Use `run: "$GITHUB_ACTION_PATH/entrypoint.sh"` instead.

Locations:

- `comment-suppress/action.yml:53`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned actions/checkout@v4 (both uses), anchore/sbom-action/download-syft@v0, sigstore/cosign-installer@v3 and github/codeql-action/upload-sarif@v3 in action.yml, and sigstore/cosign-installer@v3 in comment-suppress/action.yml. Each is now a full commit SHA from lookup_action_sha, with the tag kept as a comment. In both 'Run bomdrift' steps, `run: ${{ github.action_path }}/entrypoint.sh` became `run: "$GITHUB_ACTION_PATH/entrypoint.sh"` (shell: bash). Two edits to action.yml were lost on the first try because I made them in parallel; I redid them one at a time and read the file back to confirm. Nothing was run to test the changes.

