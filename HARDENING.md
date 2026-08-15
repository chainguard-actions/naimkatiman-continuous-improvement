<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.23.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.23.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across all workflow files use mutable version tags (`@v4`) instead of pinned 40-character commit SHA digests. This exposes the workflows to supply-chain attacks if the referenced action tags are moved or compromised. Affected references: `actions/checkout@v4` and `actions/setup-node@v4` in ci.yml, landing-drift.yml, release.yml, and skills-drift.yml.

Locations:

- `.github/workflows/ci.yml:14`
- `.github/workflows/ci.yml:16`
- `.github/workflows/ci.yml:56`
- `.github/workflows/ci.yml:57`
- `.github/workflows/landing-drift.yml:24`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:20`
- `.github/workflows/skills-drift.yml:12`

### missing-permissions (severity: medium)

ci.yml and skills-drift.yml have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, workflows inherit the default repository token permissions, which may be broader than necessary (e.g., write access to contents). A minimal `permissions: contents: read` should be added.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by pinning to full commit SHAs: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 and actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020. Added top-level `permissions: contents: read` to ci.yml and skills-drift.yml. landing-drift.yml and release.yml already had appropriate permissions blocks.

