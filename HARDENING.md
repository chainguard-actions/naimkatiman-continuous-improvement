<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.22.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.22.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files reference GitHub Actions using mutable version tags (@v4) instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references: actions/checkout@v4 and actions/setup-node@v4 in all four workflow files.

Locations:

- `.github/workflows/ci.yml:13`
- `.github/workflows/ci.yml:16`
- `.github/workflows/ci.yml:47`
- `.github/workflows/ci.yml:48`
- `.github/workflows/landing-drift.yml:24`
- `.github/workflows/release.yml:18`
- `.github/workflows/release.yml:24`
- `.github/workflows/skills-drift.yml:10`

### missing-permissions (severity: medium)

ci.yml and skills-drift.yml have no top-level permissions: block and no job-level permissions: block on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially write) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 8 unpinned action references across 4 workflow files: pinned actions/checkout@v4 to @11d5960a326750d5838078e36cf38b85af677262 and actions/setup-node@v4 to @49933ea5288caeca8642d1e84afbd3f7d6820020, preserving the original tag as a comment. Added top-level `permissions: contents: read` blocks to ci.yml and skills-drift.yml, which were missing any permissions declaration. landing-drift.yml already had `permissions: contents: read` and release.yml already had job-level permissions (contents: write, id-token: write), so those were left unchanged.

