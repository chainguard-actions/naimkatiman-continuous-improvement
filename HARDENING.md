<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.20.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.20.4** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files reference GitHub Actions using mutable version tags (@v4) instead of immutable full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks if the upstream action tag is moved to a malicious commit. Affected references: actions/checkout@v4 and actions/setup-node@v4 in ci.yml; actions/checkout@v4 and actions/setup-node@v4 in release.yml; actions/checkout@v4 in landing-drift.yml; actions/checkout@v4 in skills-drift.yml.

Locations:

- `.github/workflows/ci.yml:14`
- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:50`
- `.github/workflows/ci.yml:51`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:22`
- `.github/workflows/landing-drift.yml:22`
- `.github/workflows/skills-drift.yml:8`

### missing-permissions (severity: medium)

ci.yml has no top-level permissions block and neither of its two jobs (test, lint-transcript) defines a job-level permissions block. This means the workflow runs with the default repository permissions, which may be overly broad. Similarly, skills-drift.yml has no top-level permissions block and its only job (skills-drift) has no job-level permissions block.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full SHA digests: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 and actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 across ci.yml (4 references), release.yml (2 references), landing-drift.yml (1 reference), and skills-drift.yml (1 reference). Added top-level `permissions: contents: read` to ci.yml and skills-drift.yml. release.yml already had a job-level permissions block and landing-drift.yml already had a top-level permissions block, so those were left unchanged.

