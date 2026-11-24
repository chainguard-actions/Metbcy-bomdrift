<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v0.9.9** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml composite steps use tag refs instead of full 40-char commit SHAs: 'uses: actions/checkout@v4' (x2), 'uses: anchore/sbom-action/download-syft@v0', 'uses: sigstore/cosign-installer@v3', 'uses: github/codeql-action/upload-sarif@v3'.

Locations:

- `action.yml:299`
- `action.yml:308`
- `action.yml:321`
- `action.yml:325`
- `action.yml:384`

### unpinned-uses (severity: high)

comment-suppress/action.yml uses a tag ref instead of a commit SHA: 'uses: sigstore/cosign-installer@v3'.

Locations:

- `comment-suppress/action.yml:50`

### script-injection (severity: high)

Rule (a): the 'Run bomdrift' step puts a GitHub expression straight into the run: script: 'run: ${{ github.action_path }}/entrypoint.sh'. Use the env var instead: run: "$GITHUB_ACTION_PATH/entrypoint.sh".

Locations:

- `action.yml:329`

### script-injection (severity: high)

Rule (a): the 'Run bomdrift comment-suppress' step puts a GitHub expression straight into the run: script: 'run: ${{ github.action_path }}/entrypoint.sh'. Use "$GITHUB_ACTION_PATH/entrypoint.sh" instead.

Locations:

- `comment-suppress/action.yml:53`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned actions/checkout (x2), anchore/sbom-action/download-syft, sigstore/cosign-installer (both manifests) and github/codeql-action/upload-sarif to full commit SHAs, keeping the tag in a comment. Replaced the inline action_path expression in both run: steps with "$GITHUB_ACTION_PATH/entrypoint.sh". entrypoint.sh used GITHUB_ACTION_REF to pick which upstream Metbcy/bomdrift release to download, and on the mirror that ref is a mirror commit, so it would have fallen back to 'latest'. Pinned it to the upstream_ref v0.9.9 instead. comment-suppress/entrypoint.sh already resolves 'latest' as upstream does, so it was left unchanged.

