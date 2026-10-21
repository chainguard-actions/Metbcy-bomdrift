<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v0.9.9** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml references actions by mutable tags instead of 40-char commit SHAs: `uses: actions/checkout@v4` (twice), `uses: anchore/sbom-action/download-syft@v0`, `uses: sigstore/cosign-installer@v3`, `uses: github/codeql-action/upload-sarif@v3`.

Locations:

- `action.yml:299`
- `action.yml:308`
- `action.yml:321`
- `action.yml:325`
- `action.yml:384`

### unpinned-uses (severity: high)

comment-suppress/action.yml references `uses: sigstore/cosign-installer@v3` by a mutable tag instead of a 40-char commit SHA.

Locations:

- `comment-suppress/action.yml:50`

### script-injection (severity: high)

Sub-rule (a): a GitHub Actions expression is placed directly inside a run: block in the 'Run bomdrift' step: `run: ${{ github.action_path }}/entrypoint.sh`. Pass it through an env var instead, e.g. `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:329`

### script-injection (severity: high)

Sub-rule (a): a GitHub Actions expression is placed directly inside a run: block in the 'Run bomdrift comment-suppress' step: `run: ${{ github.action_path }}/entrypoint.sh`. Use `"$GITHUB_ACTION_PATH/entrypoint.sh"` instead.

Locations:

- `comment-suppress/action.yml:53`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned actions/checkout to 11d5960a… (v4, both uses), anchore/sbom-action/download-syft to e22c3899… (v0), sigstore/cosign-installer to 398d4b0e… (v3, in both action.yml and comment-suppress/action.yml), and github/codeql-action/upload-sarif to 4bfa242a… (v3). Each pin keeps its tag as a trailing comment. In both manifests, `run: ${{ github.action_path }}/entrypoint.sh` is now `run: '"$GITHUB_ACTION_PATH/entrypoint.sh"'` (shell: bash). I left the description text that mentions upload-sarif@v3 as it was.

