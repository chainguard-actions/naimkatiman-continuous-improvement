<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.21.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **naimkatiman--continuous-improvement/v3.21.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (@v4) instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the tag is moved. Affected references: actions/checkout@v4 and actions/setup-node@v4 in ci.yml, landing-drift.yml, release.yml, and skills-drift.yml.

Locations:

- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:20`
- `.github/workflows/ci.yml:52`
- `.github/workflows/ci.yml:53`
- `.github/workflows/landing-drift.yml:20`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:20`
- `.github/workflows/skills-drift.yml:11`

### missing-permissions (severity: medium)

ci.yml has no top-level permissions: block and neither the 'test' nor 'lint-transcript' jobs define job-level permissions. This means the workflow runs with the default (potentially broad) token permissions. Similarly, skills-drift.yml has no top-level or job-level permissions block on the 'skills-drift' job.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all actions/checkout@v4 and actions/setup-node@v4 references to full commit SHAs (checkout: 34e114876b0b11c390a56381ad16ebd13914f8d5, setup-node: 49933ea5288caeca8642d1e84afbd3f7d6820020) across ci.yml, landing-drift.yml, release.yml, and skills-drift.yml. Added top-level 'permissions: {}' and job-level 'permissions: contents: read' to ci.yml (for both 'test' and 'lint-transcript' jobs) and skills-drift.yml (for the 'skills-drift' job). The landing-drift.yml and release.yml already had appropriate permissions blocks.

