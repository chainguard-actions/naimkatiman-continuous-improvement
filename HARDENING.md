<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement--/v3.16.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **naimkatiman--continuous-improvement--/v3.16.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use action references pinned to mutable version tags (@v4) instead of immutable full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced tag is moved or compromised. Failing references: actions/checkout@v4, actions/setup-node@v4 in ci.yml; actions/checkout@v4 in landing-drift.yml; actions/checkout@v4 and actions/setup-node@v4 in release.yml; actions/checkout@v4 in skills-drift.yml.

Locations:

- `.github/workflows/ci.yml:16`
- `.github/workflows/ci.yml:18`
- `.github/workflows/ci.yml:52`
- `.github/workflows/ci.yml:53`
- `.github/workflows/landing-drift.yml:22`
- `.github/workflows/release.yml:16`
- `.github/workflows/release.yml:24`
- `.github/workflows/skills-drift.yml:11`

### missing-permissions (severity: medium)

ci.yml and skills-drift.yml have no top-level permissions: key and no job-level permissions: key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially write) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references across 4 workflow files: pinned actions/checkout@v4 to @34e114876b0b11c390a56381ad16ebd13914f8d5 and actions/setup-node@v4 to @49933ea5288caeca8642d1e84afbd3f7d6820020, with original tags preserved as comments. Added top-level `permissions: contents: read` blocks to ci.yml and skills-drift.yml. landing-drift.yml already had permissions set; release.yml already had job-level permissions (contents: write, id-token: write) appropriate for its publish workflow.

