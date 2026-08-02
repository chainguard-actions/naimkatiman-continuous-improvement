<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.22.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.22.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use mutable version tags (@v4) instead of pinned full 40-character SHA commits for their `uses:` references. This exposes the workflows to supply-chain attacks if the referenced action tags are moved or compromised.

Failing references:
- .github/workflows/ci.yml: `actions/checkout@v4` and `actions/setup-node@v4` (used in both the `test` and `lint-transcript` jobs)
- .github/workflows/release.yml: `actions/checkout@v4` and `actions/setup-node@v4`
- .github/workflows/landing-drift.yml: `actions/checkout@v4`
- .github/workflows/skills-drift.yml: `actions/checkout@v4`

All should be pinned to their full SHA digest, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:20`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:22`
- `.github/workflows/landing-drift.yml:22`
- `.github/workflows/skills-drift.yml:9`

### missing-permissions (severity: medium)

The workflow files `ci.yml` and `skills-drift.yml` have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, which may include write access to repository contents depending on the organization's default settings.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by pinning to full SHA commits: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 and actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020. Added top-level `permissions: contents: read` to ci.yml and skills-drift.yml which were missing permissions blocks. The release.yml already had job-level permissions and landing-drift.yml already had a top-level permissions block, so those only needed the SHA pinning fix.

