<!-- markdownlint-disable -->

# Hardening Report: FantasticFiasco--action-update-license-year/v3.0.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **FantasticFiasco--action-update-license-year/v3.0.4** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or branch names instead of full 40-character commit SHA pins. This exposes the workflow to supply-chain attacks if the referenced action is compromised or its tag is moved. Failing references: ci-cd.yml — actions/checkout@v6, actions/setup-node@v6, coverallsapp/github-action@master, actions/create-release@v1; greetings.yml — actions/first-interaction@v3; update-copyright-years-in-license-file.yml — actions/checkout@v6, FantasticFiasco/action-update-license-year@v3; versioning.yml — Actions-R-Us/actions-tagger@v2.

Locations:

- `.github/workflows/ci-cd.yml:12`
- `.github/workflows/ci-cd.yml:14`
- `.github/workflows/ci-cd.yml:20`
- `.github/workflows/ci-cd.yml:32`
- `.github/workflows/ci-cd.yml:33`
- `.github/workflows/greetings.yml:14`
- `.github/workflows/update-copyright-years-in-license-file.yml:10`
- `.github/workflows/update-copyright-years-in-license-file.yml:12`
- `.github/workflows/versioning.yml:9`

### missing-permissions (severity: medium)

ci-cd.yml has no top-level 'permissions:' key and neither of its jobs (build, release) defines job-level permissions. This means the workflow runs with the default, overly broad GITHUB_TOKEN permissions. All jobs should declare minimal required permissions.

Locations:

- `.github/workflows/ci-cd.yml:1`

### missing-permissions (severity: medium)

update-copyright-years-in-license-file.yml has no top-level 'permissions:' key and its only job (action-update-license-year) defines no job-level permissions. The workflow runs with default, overly broad GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/update-copyright-years-in-license-file.yml:1`

### missing-permissions (severity: medium)

versioning.yml has no top-level 'permissions:' key and its only job (actions-tagger) defines no job-level permissions. The workflow runs with default, overly broad GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 7 unpinned action references to full 40-char commit SHAs with original tag as comment: actions/checkout@v6→d23441a4, actions/setup-node@v6→249970729, coverallsapp/github-action@master→09b709cf, actions/create-release@v1→0cb9c9b6, actions/first-interaction@v3→1c4688942, FantasticFiasco/action-update-license-year@v3→f180e962, Actions-R-Us/actions-tagger@v2→330ddfac. Added top-level `permissions: {}` to ci-cd.yml, update-copyright-years-in-license-file.yml, and versioning.yml. Added job-level minimal permissions: build job gets contents:read; release job gets contents:write; action-update-license-year job gets contents:write and pull-requests:write (needed to open PRs); actions-tagger job gets contents:write (needed to manage tags).

