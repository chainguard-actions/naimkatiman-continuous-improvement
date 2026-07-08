<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.19.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **naimkatiman--continuous-improvement/v3.19.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files reference GitHub Actions using mutable version tags (@v4) instead of full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references: actions/checkout@v4 and actions/setup-node@v4.

Locations:

- `.github/workflows/ci.yml:13`
- `.github/workflows/ci.yml:16`
- `.github/workflows/ci.yml:43`
- `.github/workflows/ci.yml:44`
- `.github/workflows/landing-drift.yml:30`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:22`
- `.github/workflows/skills-drift.yml:11`

### missing-permissions (severity: medium)

ci.yml has no top-level permissions: key and neither the 'test' nor 'lint-transcript' jobs define job-level permissions. Without explicit permissions, the workflow inherits the default (write) permissions for the GITHUB_TOKEN, granting broader access than necessary.

Locations:

- `.github/workflows/ci.yml:1`

### missing-permissions (severity: medium)

skills-drift.yml has no top-level permissions: key and the 'skills-drift' job has no job-level permissions block. Without explicit permissions, the workflow inherits the default (write) permissions for the GITHUB_TOKEN.

Locations:

- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 8 unpinned action references across 4 workflow files: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 and actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, with original tags preserved as comments. Added top-level `permissions: contents: read` to ci.yml and skills-drift.yml. landing-drift.yml already had a permissions block; release.yml already had job-level permissions (contents: write, id-token: write) appropriate for publishing.

