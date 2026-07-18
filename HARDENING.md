<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.20.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.20.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All four workflow files reference GitHub Actions using mutable version tags (@v4) instead of pinned full-length SHA digests. This exposes the workflows to supply-chain attacks if the upstream action tag is moved to a malicious commit.

Failing references:
- ci.yml: `actions/checkout@v4` (line 17), `actions/setup-node@v4` (line 20), `actions/checkout@v4` (line 52), `actions/setup-node@v4` (line 53)
- release.yml: `actions/checkout@v4` (line 16), `actions/setup-node@v4` (line 27)
- landing-drift.yml: `actions/checkout@v4` (line 28)
- skills-drift.yml: `actions/checkout@v4` (line 10)

Each should be pinned to a full 40-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:20`
- `.github/workflows/ci.yml:52`
- `.github/workflows/ci.yml:53`
- `.github/workflows/release.yml:16`
- `.github/workflows/release.yml:27`
- `.github/workflows/landing-drift.yml:28`
- `.github/workflows/skills-drift.yml:10`

### missing-permissions (severity: medium)

ci.yml and skills-drift.yml have no top-level `permissions:` block and no job-level `permissions:` block on any of their jobs. Without explicit permissions, workflows inherit the repository's default token permissions (which may be read/write), violating the principle of least privilege.

- ci.yml: two jobs (`test` and `lint-transcript`) both lack permissions.
- skills-drift.yml: one job (`skills-drift`) lacks permissions.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 8 unpinned action references to full SHA digests: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 and actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, with # v4 comments for readability. Added job-level `permissions: contents: read` to the `test` and `lint-transcript` jobs in ci.yml, and to the `skills-drift` job in skills-drift.yml. The release.yml already had explicit permissions (contents: write, id-token: write) and landing-drift.yml already had a top-level permissions block, so those were left unchanged.

