<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v0.9.9** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml pins composite steps to mutable tags, not 40-char commit SHAs: `uses: actions/checkout@v4` (used twice), `uses: anchore/sbom-action/download-syft@v0`, `uses: sigstore/cosign-installer@v3`, `uses: github/codeql-action/upload-sarif@v3`.

Locations:

- `action.yml:290`
- `action.yml:298`
- `action.yml:309`
- `action.yml:313`
- `action.yml:395`

### unpinned-uses (severity: high)

comment-suppress/action.yml pins a composite step to a mutable tag: `uses: sigstore/cosign-installer@v3`.

Locations:

- `comment-suppress/action.yml:50`

### script-injection (severity: high)

Rule (a): a GitHub expression is placed straight into a run: block in the 'Run bomdrift' step: `run: ${{ github.action_path }}/entrypoint.sh`. Fix: use the shell env var instead, quoted: `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `action.yml:316`

### script-injection (severity: high)

Rule (a): a GitHub expression is placed straight into a run: block in the 'Run bomdrift comment-suppress' step: `run: ${{ github.action_path }}/entrypoint.sh`. Fix: use `run: "$GITHUB_ACTION_PATH/entrypoint.sh"`.

Locations:

- `comment-suppress/action.yml:53`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned actions/checkout (x2), anchore/sbom-action/download-syft, sigstore/cosign-installer (both manifests), and github/codeql-action/upload-sarif to full commit SHAs with tag comments. Replaced inline ${{ github.action_path }}/entrypoint.sh with quoted "$GITHUB_ACTION_PATH/entrypoint.sh" in both action.yml and comment-suppress/action.yml (bash shell).

