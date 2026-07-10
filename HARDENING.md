<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.20.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **naimkatiman--continuous-improvement/v3.20.4** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (@v4) instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the tag is moved. Affected references: actions/checkout@v4 and actions/setup-node@v4 in ci.yml, landing-drift.yml, release.yml, and skills-drift.yml.

Locations:

- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:20`
- `.github/workflows/ci.yml:57`
- `.github/workflows/ci.yml:58`
- `.github/workflows/landing-drift.yml:22`
- `.github/workflows/release.yml:16`
- `.github/workflows/release.yml:24`
- `.github/workflows/skills-drift.yml:10`

### missing-permissions (severity: medium)

ci.yml has no top-level permissions block and no job-level permissions block on any of its jobs (test, lint-transcript). skills-drift.yml has no top-level permissions block and no job-level permissions block on its skills-drift job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially write) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all actions/checkout@v4 references to @34e114876b0b11c390a56381ad16ebd13914f8d5 and all actions/setup-node@v4 references to @49933ea5288caeca8642d1e84afbd3f7d6820020 across ci.yml (4 pins), landing-drift.yml (1 pin), release.yml (2 pins), and skills-drift.yml (1 pin). Added top-level `permissions: contents: read` blocks to ci.yml and skills-drift.yml. landing-drift.yml already had `permissions: contents: read` and release.yml already had job-level permissions.

