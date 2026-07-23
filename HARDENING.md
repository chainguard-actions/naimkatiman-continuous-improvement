<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.9.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.9.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference GitHub Actions using mutable version tags (@v4) instead of immutable full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the upstream action tag is moved or compromised. Failing references: actions/checkout@v4, actions/setup-node@v4.

Locations:

- `.github/workflows/ci.yml:13`
- `.github/workflows/ci.yml:16`
- `.github/workflows/ci.yml:52`
- `.github/workflows/ci.yml:53`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:18`
- `.github/workflows/skills-drift.yml:10`

### missing-permissions (severity: medium)

ci.yml and skills-drift.yml have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions, which may be overly broad (e.g., write access to contents). Explicit minimal permissions should be declared.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across three workflow files by resolving mutable @v4 tags to full commit SHAs: actions/checkout → 11d5960a326750d5838078e36cf38b85af677262, actions/setup-node → 49933ea5288caeca8642d1e84afbd3f7d6820020. Added top-level `permissions: contents: read` to ci.yml and skills-drift.yml. release.yml already had appropriate job-level permissions (contents: write, id-token: write) for its publish workflow.

