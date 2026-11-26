<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/0.9.7-alpha

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/0.9.7-alpha** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml composite steps reference actions by mutable tag instead of a 40-char commit SHA: 'uses: actions/checkout@v4' (x2), 'uses: anchore/sbom-action/download-syft@v0', 'uses: sigstore/cosign-installer@v3', 'uses: github/codeql-action/upload-sarif@v3'.

Locations:

- `action.yml:329`

### unpinned-uses (severity: high)

comment-suppress/action.yml references 'uses: sigstore/cosign-installer@v3' by mutable tag instead of a full commit SHA.

Locations:

- `comment-suppress/action.yml:50`

### script-injection (severity: high)

Rule (a): the 'Run bomdrift' step puts an expression directly in the run: string: 'run: ${{ github.action_path }}/entrypoint.sh'. Pass it through env instead, e.g. run: "$GITHUB_ACTION_PATH/entrypoint.sh".

Locations:

- `action.yml:358`

### script-injection (severity: high)

Rule (a): the 'Run bomdrift comment-suppress' step puts an expression directly in the run: string: 'run: ${{ github.action_path }}/entrypoint.sh'. Use "$GITHUB_ACTION_PATH/entrypoint.sh" instead.

Locations:

- `comment-suppress/action.yml:53`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

I pinned every `uses:` reference to a full commit SHA, looked up with lookup_action_sha. The tag is kept as a trailing comment.
- actions/checkout@11d5960a… # v4 (both checkouts)
- anchore/sbom-action/download-syft@e22c3899… # v0
- sigstore/cosign-installer@398d4b0e… # v3, in action.yml and comment-suppress/action.yml
- github/codeql-action/upload-sarif@74c1aea9… # v3

The action's input descriptions also mention `upload-sarif@v3`. That is plain text, so I left it alone.

In both action.yml and comment-suppress/action.yml, I replaced `run: ${{ github.action_path }}/entrypoint.sh` with `run: '"$GITHUB_ACTION_PATH/entrypoint.sh"'`. Both steps use `shell: bash`, so this works as written.

One change goes beyond the findings. In entrypoint.sh, `resolve_tag` uses `GITHUB_ACTION_REF` to pick which bomdrift release to download from Metbcy/bomdrift. On the mirror, that ref names a mirror tag or commit, not an upstream release. I set it to the upstream ref, `0.9.7-alpha`. This does not change what gets downloaded: that ref doesn't match the script's `vX.Y.Z` pattern, so it falls back to the latest release, which is what upstream does at this tag too.

Nothing was run or tested.

