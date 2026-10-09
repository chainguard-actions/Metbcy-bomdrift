<!-- markdownlint-disable -->

# Hardening Report: Metbcy--bomdrift/0.9.7-alpha

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Metbcy--bomdrift/0.9.7-alpha** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in action.yml and comment-suppress/action.yml are pinned to mutable tags rather than immutable 40-character commit SHAs, making the action vulnerable to supply-chain attacks if any of those upstream actions are compromised or their tags are moved.

Failing references in action.yml:
- `uses: actions/checkout@v4` (appears twice)
- `uses: anchore/sbom-action/download-syft@v0`
- `uses: sigstore/cosign-installer@v3`
- `uses: github/codeql-action/upload-sarif@v3`

Failing references in comment-suppress/action.yml:
- `uses: sigstore/cosign-installer@v3`

All should be pinned to full SHA digests, e.g. `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `action.yml:285`
- `action.yml:293`
- `action.yml:301`
- `action.yml:305`
- `action.yml:363`
- `comment-suppress/action.yml:53`

### script-injection (severity: high)

Sub-rule (a): A `${{ ... }}` expression is interpolated directly inside a `run:` shell command string in both action.yml and comment-suppress/action.yml.

Offending lines:
- `run: ${{ github.action_path }}/entrypoint.sh`

Although `github.action_path` is set by the runner and is not directly attacker-controlled, the check rules require that NO `${{ ... }}` expression appear anywhere inside a `run:` shell command string. The value flows through YAML template substitution before the shell processes it. The safe alternative is to use the equivalent environment variable `$GITHUB_ACTION_PATH` which is always set by the runner:
```yaml
run: "$GITHUB_ACTION_PATH/entrypoint.sh"
```

Locations:

- `action.yml:309`
- `comment-suppress/action.yml:55`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all 6 unpinned `uses:` references by resolving each tag to its full 40-character commit SHA using lookup_action_sha. Fixed both script-injection findings by replacing `${{ github.action_path }}/entrypoint.sh` with `$GITHUB_ACTION_PATH/entrypoint.sh` in both action.yml and comment-suppress/action.yml.

