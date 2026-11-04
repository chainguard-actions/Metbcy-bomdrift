<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/v0.9.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/v0.9.9** was hardened automatically. 4 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml points to actions by version tag instead of a full 40-character commit SHA: `uses: actions/checkout@v4` (twice), `uses: anchore/sbom-action/download-syft@v0`, `uses: sigstore/cosign-installer@v3`, `uses: github/codeql-action/upload-sarif@v3`.

Locations:

- `action.yml:296`
- `action.yml:305`
- `action.yml:316`
- `action.yml:320`
- `action.yml:384`

### unpinned-uses (severity: high)

comment-suppress/action.yml points to an action by version tag instead of a commit SHA: `uses: sigstore/cosign-installer@v3`.

Locations:

- `comment-suppress/action.yml:48`

### script-injection (severity: high)

Sub-rule (a): a GitHub expression is placed directly inside a run: script: `run: ${{ github.action_path }}/entrypoint.sh` in the 'Run bomdrift' step. Pass it through an env var or use the quoted "$GITHUB_ACTION_PATH/entrypoint.sh" instead.

Locations:

- `action.yml:324`

### script-injection (severity: high)

Sub-rule (a): a GitHub expression is placed directly inside a run: script: `run: ${{ github.action_path }}/entrypoint.sh` in the 'Run bomdrift comment-suppress' step. Use the quoted "$GITHUB_ACTION_PATH/entrypoint.sh" instead.

Locations:

- `comment-suppress/action.yml:51`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned actions/checkout (twice), anchore/sbom-action/download-syft, sigstore/cosign-installer (action.yml and comment-suppress/action.yml) and github/codeql-action/upload-sarif to full SHAs, keeping the tag in a trailing comment. Changed both 'run: ${{ github.action_path }}/entrypoint.sh' steps to run "$GITHUB_ACTION_PATH/entrypoint.sh". Both steps use shell: bash.

### Iteration 2

**Fixes applied:** unpinned-uses, script-injection, invalid-yaml

**Notes:**

I repaired three steps in action.yml that an earlier edit had spliced into the wrong lines. I checked the result by reading the file only; I didn't run a YAML parser or the act tests.

1. **Install Syft:** the step's name, `if:` and `uses:` are now separate lines. It is pinned to anchore/sbom-action/download-syft@e22c389904149dbc22b58101806040fa8d37a610 # v0. The step name ("Install Syft (for in-action SBOM generation)") and the condition `inputs.before-sbom == '' || inputs.after-sbom == ''` were partly overwritten, so I filled them in from the leftover text and the comment above the step.

2. **Run bomdrift:** this step had lost its `- name:` and `shell: bash` lines, so I added them back. The run line is now `run: '"$GITHUB_ACTION_PATH/entrypoint.sh"'`, with no `${{ github.action_path }}` left on it.

3. **Upload SARIF:** the condition is now `inputs.upload-to-code-scanning == 'true' && inputs.output == 'sarif'`. I rebuilt the cut-off part of it from the step's comment, which says to upload only when SARIF output is set and the upload is turned on. It is pinned to github/codeql-action/upload-sarif@9418214df1f2c4b8128c770156c409013b07d6b9 # v3.

The repaired names and conditions are my best reconstruction, not upstream's exact text. Worth checking them against the original v0.9.9 action.yml if you have it.

