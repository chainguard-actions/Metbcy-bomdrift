<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v1** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml composite steps reference actions by mutable tags instead of 40-char commit SHAs: 'uses: actions/checkout@v4' (twice), 'uses: anchore/sbom-action/download-syft@v0', 'uses: sigstore/cosign-installer@v3', 'uses: github/codeql-action/upload-sarif@v3'.

Locations:

- `action.yml:303`
- `action.yml:312`
- `action.yml:324`
- `action.yml:328`
- `action.yml:389`

### unpinned-uses (severity: high)

comment-suppress/action.yml references 'uses: sigstore/cosign-installer@v3' by mutable tag instead of a full commit SHA.

Locations:

- `comment-suppress/action.yml:49`

### script-injection (severity: high)

Rule (a): the 'Run bomdrift' step puts a GitHub Actions expression straight into the run: string: 'run: ${{ github.action_path }}/entrypoint.sh'. Pass the path through an env variable or use "$GITHUB_ACTION_PATH/entrypoint.sh" (double-quoted) instead.

Locations:

- `action.yml:332`

### script-injection (severity: high)

Rule (a): the 'Run bomdrift comment-suppress' step puts a GitHub Actions expression straight into the run: string: 'run: ${{ github.action_path }}/entrypoint.sh'. Use "$GITHUB_ACTION_PATH/entrypoint.sh" (double-quoted) instead.

Locations:

- `comment-suppress/action.yml:52`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned actions/checkout (x2), anchore/sbom-action/download-syft, sigstore/cosign-installer (in both manifests) and github/codeql-action/upload-sarif to full commit SHAs, keeping the tag as a comment. Replaced run: ${{ github.action_path }}/entrypoint.sh with a double-quoted "$GITHUB_ACTION_PATH/entrypoint.sh" in action.yml and comment-suppress/action.yml. Both steps use shell: bash.

