<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement--/v3.17.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **naimkatiman--continuous-improvement--/v3.17.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use action references pinned to mutable tags (e.g. @v4) rather than immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved. Failing references:
- .github/workflows/ci.yml: actions/checkout@v4, actions/setup-node@v4 (used in both 'test' and 'lint-transcript' jobs)
- .github/workflows/landing-drift.yml: actions/checkout@v4
- .github/workflows/release.yml: actions/checkout@v4, actions/setup-node@v4
- .github/workflows/skills-drift.yml: actions/checkout@v4

Locations:

- `.github/workflows/ci.yml:14`
- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:50`
- `.github/workflows/ci.yml:51`
- `.github/workflows/landing-drift.yml:22`
- `.github/workflows/release.yml:17`
- `.github/workflows/release.yml:27`
- `.github/workflows/skills-drift.yml:10`

### missing-permissions (severity: medium)

ci.yml has no top-level 'permissions:' key and neither of its jobs ('test', 'lint-transcript') defines job-level permissions. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions. Similarly, skills-drift.yml has no top-level or job-level 'permissions:' key on its 'skills-drift' job.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references across 4 workflow files by pinning to full commit SHAs: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 and actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020. Added top-level `permissions: contents: read` to ci.yml and skills-drift.yml which were missing permissions blocks. landing-drift.yml already had `permissions: contents: read` and release.yml already had job-level permissions (contents: write, id-token: write), so those did not need permissions changes.

