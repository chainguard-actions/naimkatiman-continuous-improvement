<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.16.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.16.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All four workflow files reference actions using mutable version tags (@v4) instead of pinned 40-character commit SHA hashes. This exposes the workflows to supply-chain attacks if the upstream action tags are moved or compromised. Affected references: actions/checkout@v4 and actions/setup-node@v4 in all four files.

Locations:

- `.github/workflows/ci.yml:12`
- `.github/workflows/ci.yml:15`
- `.github/workflows/ci.yml:50`
- `.github/workflows/ci.yml:52`
- `.github/workflows/landing-drift.yml:27`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:21`
- `.github/workflows/skills-drift.yml:10`

### missing-permissions (severity: medium)

ci.yml has no top-level permissions: key and no job-level permissions: key on any of its jobs (test, lint-transcript). Without explicit permissions, the workflow inherits the repository default, which may be write-all for private repos or read-all for public repos — both are overly broad. Minimal permissions (e.g. contents: read) should be declared.

Locations:

- `.github/workflows/ci.yml:1`

### missing-permissions (severity: medium)

skills-drift.yml has no top-level permissions: key and no job-level permissions: key on its skills-drift job. Without explicit permissions, the workflow inherits the repository default. Minimal permissions (e.g. contents: read) should be declared.

Locations:

- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 8 unpinned action references across 4 workflow files by replacing @v4 tags with full commit SHAs: actions/checkout pinned to 34e114876b0b11c390a56381ad16ebd13914f8d5 and actions/setup-node pinned to 49933ea5288caeca8642d1e84afbd3f7d6820020. Added top-level `permissions: contents: read` to ci.yml and skills-drift.yml. landing-drift.yml already had permissions declared; release.yml already had job-level permissions with contents: write and id-token: write for its publish job.

