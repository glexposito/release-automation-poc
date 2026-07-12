# release-automation-poc

POC for automating release branch cuts from GitHub to an Azure DevOps build/release pipeline.

## Flow

1. **`main`** — primary dev branch.
2. On every push to `main`, [`sync-minor-from-main.yml`](.github/workflows/sync-minor-from-main.yml) force-mirrors it onto **`minor`**.
3. The team merges PRs directly into `minor`.
4. On every push to `minor`, [`create-release-branch.yml`](.github/workflows/create-release-branch.yml) ("Open team release") bumps the minor version from the latest `vX.Y.Z` tag and cuts **`release/<major>.<minor+1>.0`** from it. Fails if that branch already exists rather than overwriting it.
5. In Azure DevOps: a build pipeline CI-triggers on `release/*`, and a release pipeline auto-creates a deployment from that build via a continuous deployment trigger filtered to `release/*`.

## Notes

- Version tags (`vX.Y.Z`) are created by whoever closes out a release — not by this automation.
- The build/release links posted in the job summary point to the general ADO build/release lists (no definition IDs wired up yet).
