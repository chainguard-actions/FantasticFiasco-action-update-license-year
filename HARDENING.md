<!-- markdownlint-disable -->

# Hardening Report: FantasticFiasco--action-update-license-year/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **FantasticFiasco--action-update-license-year/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions by mutable tags or branch names instead of full 40-character commit SHAs, making the workflows vulnerable to supply-chain attacks if those tags/branches are moved.

- ci-cd.yml: `actions/checkout@v3`, `actions/setup-node@v3`, `coverallsapp/github-action@master`, `actions/create-release@v1`
- greetings.yml: `actions/first-interaction@v1`
- update-copyright-years-in-license-file.yml: `actions/checkout@v3`, `FantasticFiasco/action-update-license-year@v2`
- versioning.yml: `Actions-R-Us/actions-tagger@v2`

Locations:

- `.github/workflows/ci-cd.yml:12`
- `.github/workflows/ci-cd.yml:14`
- `.github/workflows/ci-cd.yml:19`
- `.github/workflows/ci-cd.yml:29`
- `.github/workflows/greetings.yml:13`
- `.github/workflows/update-copyright-years-in-license-file.yml:10`
- `.github/workflows/update-copyright-years-in-license-file.yml:12`
- `.github/workflows/versioning.yml:9`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` key and no job-level `permissions:` on any of their jobs. Without explicit permissions, workflows run with the default (potentially broad) token permissions.

- ci-cd.yml: no permissions block at top level or on the `build` or `release` jobs
- update-copyright-years-in-license-file.yml: no permissions block
- versioning.yml: no permissions block

Locations:

- `.github/workflows/ci-cd.yml:1`
- `.github/workflows/update-copyright-years-in-license-file.yml:1`
- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 4 workflow files:

1. ci-cd.yml: Pinned actions/checkout@v3→SHA, actions/setup-node@v3→SHA, coverallsapp/github-action@master→SHA, actions/create-release@v1→SHA. Added top-level `permissions: {}` with job-level overrides: `contents: read` for build job, `contents: write` for release job.

2. greetings.yml: Pinned actions/first-interaction@v1→SHA. Permissions block (issues: write) was already present.

3. update-copyright-years-in-license-file.yml: Pinned actions/checkout@v3→SHA, FantasticFiasco/action-update-license-year@v2→SHA. Added `permissions: contents: write, pull-requests: write` (needed for the action to commit changes and open PRs).

4. versioning.yml: Pinned Actions-R-Us/actions-tagger@v2→SHA. Added `permissions: contents: write` (needed for the tagger to create/update tags).

