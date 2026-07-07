<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.18.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **naimkatiman--continuous-improvement/v3.18.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (@v4) instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references: actions/checkout@v4 and actions/setup-node@v4 in all four workflow files.

Locations:

- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:20`
- `.github/workflows/ci.yml:57`
- `.github/workflows/ci.yml:58`
- `.github/workflows/landing-drift.yml:20`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:21`
- `.github/workflows/skills-drift.yml:8`

### missing-permissions (severity: medium)

ci.yml and skills-drift.yml have no top-level permissions: key and no job-level permissions: key on any of their jobs. Without explicit permissions, workflows inherit the default repository permissions (which may include write access), violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references across 4 workflow files: replaced actions/checkout@v4 with @34e114876b0b11c390a56381ad16ebd13914f8d5 and actions/setup-node@v4 with @49933ea5288caeca8642d1e84afbd3f7d6820020 (both with # v4 comments). Added top-level 'permissions: contents: read' to ci.yml and skills-drift.yml which were missing permissions blocks. landing-drift.yml and release.yml already had permissions blocks.

