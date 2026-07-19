<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.17.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.17.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across the workflow files use mutable version tags (e.g. `@v4`) instead of immutable 40-character SHA commit digests. This exposes the workflows to supply-chain attacks if the referenced action tags are moved or compromised. Failing references: `actions/checkout@v4` and `actions/setup-node@v4` in ci.yml; `actions/checkout@v4` and `actions/setup-node@v4` in release.yml; `actions/checkout@v4` in landing-drift.yml; `actions/checkout@v4` in skills-drift.yml.

Locations:

- `.github/workflows/ci.yml:14`
- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:47`
- `.github/workflows/ci.yml:48`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:21`
- `.github/workflows/landing-drift.yml:22`
- `.github/workflows/skills-drift.yml:9`

### missing-permissions (severity: medium)

The workflow files `ci.yml` and `skills-drift.yml` have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially write) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full SHA digests: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 and actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, with # v4 comments for readability. Applied to ci.yml (4 occurrences), release.yml (2 occurrences), landing-drift.yml (1 occurrence), and skills-drift.yml (1 occurrence). Added top-level `permissions: contents: read` to ci.yml and skills-drift.yml, which had no permissions blocks. landing-drift.yml and release.yml already had appropriate permissions.

