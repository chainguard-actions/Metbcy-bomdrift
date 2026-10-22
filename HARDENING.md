<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v1** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml refers to actions by mutable tags instead of 40-character commit SHAs: `uses: actions/checkout@v4` (twice), `uses: anchore/sbom-action/download-syft@v0`, `uses: sigstore/cosign-installer@v3` and `uses: github/codeql-action/upload-sarif@v3`.

Locations:

- `action.yml:272`
- `action.yml:281`
- `action.yml:293`
- `action.yml:297`
- `action.yml:370`

### unpinned-uses (severity: high)

comment-suppress/action.yml refers to an action by a mutable tag instead of a commit SHA: `uses: sigstore/cosign-installer@v3`.

Locations:

- `comment-suppress/action.yml:51`

### script-injection (severity: high)

Sub-rule (a): a GitHub Actions expression is placed directly in the run: block of the 'Run bomdrift' step: `run: ${{ github.action_path }}/entrypoint.sh`. Pass it through an env var instead and double-quote it, for example `"$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:301`

### script-injection (severity: high)

Sub-rule (a): a GitHub Actions expression is placed directly in the run: block of the 'Run bomdrift comment-suppress' step: `run: ${{ github.action_path }}/entrypoint.sh`. Use `"$GITHUB_ACTION_PATH/entrypoint.sh"` instead.

Locations:

- `comment-suppress/action.yml:54`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned actions/checkout@v4 (both uses), anchore/sbom-action/download-syft@v0, sigstore/cosign-installer@v3 (in action.yml and comment-suppress/action.yml) and github/codeql-action/upload-sarif@v3 to full commit SHAs, with the tag kept as a comment. In both action.yml files, changed `run: ${{ github.action_path }}/entrypoint.sh` to `run: '"$GITHUB_ACTION_PATH/entrypoint.sh"'` (shell: bash). Some edits garbled parts of action.yml: step headers for the cosign and SARIF steps got cut off or duplicated. I fixed those sections by hand and read the final file back to confirm it is right.

