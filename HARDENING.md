<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/0.9.7-alpha

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/0.9.7-alpha** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command string. In action.yml, the step 'Run bomdrift' uses `run: ${{ github.action_path }}/entrypoint.sh` — the github.action_path context is substituted directly into the shell command before the shell parses it. While github.action_path is not attacker-controlled in the same way as github.head_ref, any ${{ ... }} expression inside a run: block is a script-injection finding per the check rules. The same pattern appears in comment-suppress/action.yml for the 'Run bomdrift comment-suppress' step.

Locations:

- `action.yml:243`
- `comment-suppress/action.yml:47`

### unpinned-uses (severity: high)

Multiple uses: references in action.yml and comment-suppress/action.yml are pinned to mutable tags rather than full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the referenced tags are moved or compromised. Failing references in action.yml: `actions/checkout@v4`, `actions/checkout@v4` (second checkout step), `anchore/sbom-action/download-syft@v0`, `sigstore/cosign-installer@v3`, `github/codeql-action/upload-sarif@v3`. Failing reference in comment-suppress/action.yml: `sigstore/cosign-installer@v3`.

Locations:

- `action.yml:233`
- `action.yml:240`
- `action.yml:247`
- `action.yml:252`
- `action.yml:310`
- `comment-suppress/action.yml:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script-injection in action.yml and comment-suppress/action.yml by moving `${{ github.action_path }}` out of run: commands into env: blocks as ACTION_PATH, then referencing via $ACTION_PATH in the shell. Pinned all 6 unpinned uses references to full commit SHAs: actions/checkout@v4→11d5960a3, anchore/sbom-action/download-syft@v0→e22c389904, sigstore/cosign-installer@v3→398d4b0eee (both files), github/codeql-action/upload-sarif@v3→ce5e7eeac3. Original tag names preserved as inline comments.

