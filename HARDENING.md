<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.21.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.21.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (e.g. @v4) instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the upstream action tag is moved or compromised. Affected references: actions/checkout@v4 and actions/setup-node@v4 in ci.yml; actions/checkout@v4 and actions/setup-node@v4 in release.yml; actions/checkout@v4 in landing-drift.yml; actions/checkout@v4 in skills-drift.yml.

Locations:

- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:20`
- `.github/workflows/release.yml:12`
- `.github/workflows/release.yml:18`
- `.github/workflows/landing-drift.yml:24`
- `.github/workflows/skills-drift.yml:10`

### missing-permissions (severity: medium)

ci.yml and skills-drift.yml have no top-level 'permissions:' key and no job-level 'permissions:' key on any of their jobs. Without explicit permissions, workflows inherit the repository's default token permissions (which may be read/write), violating the principle of least privilege. Each workflow should declare minimal required permissions.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all mutable action references to full commit SHAs: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 and actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, with # v4 comments for readability. Added top-level 'permissions: contents: read' to ci.yml and skills-drift.yml. release.yml already had job-level permissions; landing-drift.yml already had top-level permissions.

