<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.25.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.25.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All four workflow files use action references pinned to mutable version tags (@v4) rather than immutable 40-character commit SHAs. This exposes the workflows to supply-chain attacks if the upstream action tags are moved. Affected references:
- ci.yml: actions/checkout@v4, actions/setup-node@v4 (used twice, in both jobs)
- release.yml: actions/checkout@v4, actions/setup-node@v4
- landing-drift.yml: actions/checkout@v4
- skills-drift.yml: actions/checkout@v4

Locations:

- `.github/workflows/ci.yml:13`
- `.github/workflows/ci.yml:16`
- `.github/workflows/ci.yml:53`
- `.github/workflows/ci.yml:54`
- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:22`
- `.github/workflows/landing-drift.yml:22`
- `.github/workflows/skills-drift.yml:9`

### missing-permissions (severity: medium)

ci.yml has no top-level `permissions:` key and neither of its jobs (test, lint-transcript) defines a job-level `permissions:` block. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions. Similarly, skills-drift.yml has no top-level or job-level `permissions:` block on its skills-drift job.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 8 unpinned action references across 4 workflow files by pinning to full commit SHAs: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 and actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020. Original tags preserved as inline comments. Added top-level `permissions: contents: read` blocks to ci.yml and skills-drift.yml, which only need read access to repository contents. release.yml already had a job-level permissions block (contents: write, id-token: write) and landing-drift.yml already had a top-level permissions block (contents: read), so those were left unchanged.

