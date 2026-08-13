<!-- markdownlint-disable -->

# Hardening Report: FantasticFiasco--action-update-license-year/v3.0.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **FantasticFiasco--action-update-license-year/v3.0.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions by mutable tags or branch names instead of pinned full-length SHA commit hashes, making them vulnerable to supply-chain attacks.

- ci-cd.yml: `actions/checkout@v4`, `actions/setup-node@v4`, `coverallsapp/github-action@master`, `actions/create-release@v1`
- greetings.yml: `actions/first-interaction@v1`
- update-copyright-years-in-license-file.yml: `actions/checkout@v4`, `FantasticFiasco/action-update-license-year@v3`
- versioning.yml: `Actions-R-Us/actions-tagger@v2`

Locations:

- `.github/workflows/ci-cd.yml:12`
- `.github/workflows/ci-cd.yml:14`
- `.github/workflows/ci-cd.yml:20`
- `.github/workflows/ci-cd.yml:31`
- `.github/workflows/greetings.yml:13`
- `.github/workflows/update-copyright-years-in-license-file.yml:10`
- `.github/workflows/update-copyright-years-in-license-file.yml:12`
- `.github/workflows/versioning.yml:10`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` block and no job-level `permissions:` block on any job. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions.

- ci-cd.yml: neither the workflow nor its `build` or `release` jobs declare permissions.
- update-copyright-years-in-license-file.yml: neither the workflow nor its job declares permissions.
- versioning.yml: neither the workflow nor its job declares permissions.

Locations:

- `.github/workflows/ci-cd.yml:1`
- `.github/workflows/update-copyright-years-in-license-file.yml:1`
- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 8 unpinned action references to full commit SHAs with original tags preserved as comments:
- actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5
- actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020
- coverallsapp/github-action@master → @09b709cf6a16e30b0808ba050c7a6e8a5ef13f8d
- actions/create-release@v1 → @0cb9c9b65d5d1901c1f53e5e66eaf4afd303e70e
- actions/first-interaction@v1 → @2ec0f0fd78838633cd1c1342e4536d49ef72be54
- FantasticFiasco/action-update-license-year@v3 → @f180e962fa988db222d8f03ef4636750312d1b3d
- Actions-R-Us/actions-tagger@v2 → @330ddfac760021349fef7ff62b372f2f691c20fb

Added permissions blocks to ci-cd.yml, update-copyright-years-in-license-file.yml, and versioning.yml:
- Top-level `permissions: {}` to deny all by default
- Job-level minimal permissions: `contents: read` for build job, `contents: write` for release/tagger/license-update jobs, `pull-requests: write` for license-update job (needed to open PRs)

