<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.24.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.24.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All four workflow files reference GitHub Actions using mutable version tags (@v4) instead of pinned full-length SHA commits. This exposes the workflow to supply-chain attacks if the upstream action tag is moved to a malicious commit. Affected references: actions/checkout@v4 and actions/setup-node@v4 in all files.

Locations:

- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:20`
- `.github/workflows/ci.yml:55`
- `.github/workflows/ci.yml:56`
- `.github/workflows/landing-drift.yml:22`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:20`
- `.github/workflows/skills-drift.yml:10`

### missing-permissions (severity: medium)

ci.yml has no top-level permissions: block and neither of its two jobs (test, lint-transcript) defines job-level permissions. skills-drift.yml has no top-level permissions: block and its only job (skills-drift) has no job-level permissions. Without explicit permissions, workflows inherit the default repository token permissions, which may be overly broad (e.g., write access to contents).

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 8 unpinned action references across 4 workflow files by pinning to full commit SHAs (actions/checkout@11d5960a326750d5838078e36cf38b85af677262 and actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020), preserving the original tag in a comment. Added top-level `permissions: contents: read` blocks to ci.yml and skills-drift.yml which were missing them. landing-drift.yml already had a permissions block, and release.yml already had job-level permissions defined.

