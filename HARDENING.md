<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v0.9.9** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml composite steps reference actions by mutable tag rather than full commit SHA: 'uses: actions/checkout@v4' (twice), 'uses: anchore/sbom-action/download-syft@v0', 'uses: sigstore/cosign-installer@v3', 'uses: github/codeql-action/upload-sarif@v3'.

Locations:

- `action.yml:307`
- `action.yml:316`
- `action.yml:327`
- `action.yml:331`
- `action.yml:395`

### unpinned-uses (severity: high)

comment-suppress/action.yml references 'uses: sigstore/cosign-installer@v3' by mutable tag rather than full commit SHA.

Locations:

- `comment-suppress/action.yml:50`

### script-injection (severity: high)

Rule (a): the 'Run bomdrift' step puts a GitHub expression straight into the run: command: 'run: ${{ github.action_path }}/entrypoint.sh'. The rule fails any ${{ }} expression inside run:. Fix: use "$GITHUB_ACTION_PATH/entrypoint.sh" instead.

Locations:

- `action.yml:335`

### script-injection (severity: high)

Rule (a): the 'Run bomdrift comment-suppress' step puts a GitHub expression straight into the run: command: 'run: ${{ github.action_path }}/entrypoint.sh'. Fix: use "$GITHUB_ACTION_PATH/entrypoint.sh" instead.

Locations:

- `comment-suppress/action.yml:53`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned every action to its full commit SHA, keeping the tag as a trailing comment: actions/checkout v4 (11d5960a…, used twice), anchore/sbom-action/download-syft v0 (e22c3899…), sigstore/cosign-installer v3 (398d4b0e…, in both action.yml and comment-suppress/action.yml) and github/codeql-action/upload-sarif v3 (74c1aea9…). In both manifests, changed 'run: ${{ github.action_path }}/entrypoint.sh' to run "$GITHUB_ACTION_PATH/entrypoint.sh" (shell is bash). Also fixed one thing that wasn't in the findings: resolve_tag() in entrypoint.sh chose which upstream Metbcy/bomdrift release to download from GITHUB_ACTION_REF. On the mirror that is a mirror ref, so it would fall back to the latest release instead of the matching one. It is now pinned to the upstream_ref 'v0.9.9'. comment-suppress/entrypoint.sh downloads the latest release, exactly as upstream does, so I left it unchanged.

