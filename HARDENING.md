<!-- markdownlint-disable -->

# Hardening Report: naimkatiman--continuous-improvement/v3.12.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **naimkatiman--continuous-improvement/v3.12.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use mutable version tags instead of pinned full-length SHA commit hashes for their `uses:` references, making them vulnerable to supply-chain attacks if the referenced action tags are moved.

**ci.yml**: `actions/checkout@v4`, `actions/setup-node@v4` (×2)
**cf-pages.yml**: `actions/checkout@v4`, `cloudflare/wrangler-action@v3`
**pages.yml**: `actions/checkout@v4`, `actions/configure-pages@v5`, `actions/upload-pages-artifact@v3`, `actions/deploy-pages@v4`
**release.yml**: `actions/checkout@v4`, `actions/setup-node@v4`
**skills-drift.yml**: `actions/checkout@v4`

All should be replaced with full 40-character hex SHA digests (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

Locations:

- `.github/workflows/ci.yml:14`
- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:50`
- `.github/workflows/ci.yml:53`
- `.github/workflows/cf-pages.yml:19`
- `.github/workflows/cf-pages.yml:23`
- `.github/workflows/pages.yml:20`
- `.github/workflows/pages.yml:23`
- `.github/workflows/pages.yml:27`
- `.github/workflows/pages.yml:32`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:22`
- `.github/workflows/skills-drift.yml:12`

### missing-permissions (severity: medium)

Two workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, violating the principle of least privilege.

- `.github/workflows/ci.yml`: No permissions block at top-level or in the `test` or `lint-transcript` jobs.
- `.github/workflows/skills-drift.yml`: No permissions block at top-level or in the `skills-drift` job.

Add a top-level `permissions: read-all` (or more specific scopes such as `contents: read`) to restrict the token to the minimum required.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/skills-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 13 unpinned action references across 5 workflow files to full 40-char SHA digests:
- actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262
- actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020
- cloudflare/wrangler-action@v3 → @9acf94ace14e7dc412b076f2c5c20b8ce93c79cd
- actions/configure-pages@v5 → @983d7736d9b0ae728b81ab479565c72886d7745b
- actions/upload-pages-artifact@v3 → @56afc609e74202658d3ffba0e8f6dda462b719fa
- actions/deploy-pages@v4 → @d6db90164ac5ed86f2b6aed7e0febac5b3c0c03e

Added top-level `permissions: contents: read` to ci.yml and skills-drift.yml. The other three workflow files (cf-pages.yml, pages.yml, release.yml) already had permissions blocks.

