# novolis-workflows

Shared GitHub Actions for Novolis package repos. Repo workflows are thin triggers; logic lives here.

## Reusable workflows

| Workflow | Repo file | Purpose |
|----------|-----------|---------|
| `dotnet-pull-request.yml` | `pull-request.yml` | Restore, build, test |
| `dotnet-merge-publish.yml` | `merge.yml` | Above + pack + GitHub Packages + version bump |
| `dotnet-release-publish.yml` | `release.yml` | Pack at tag, nuget.org, attach assets to release |
| `dotnet-raylib-pull-request.yml` | `novolis-raylib` PR | Raylib multi-project CI |
| `dotnet-raylib-merge-publish.yml` | `novolis-raylib` merge | Raylib CI + pack/publish |

## Composite actions (building blocks)

| Action | Role |
|--------|------|
| `dotnet-build` | SDK setup, optional GPR auth, restore/build/test |
| `pack` | `dotnet pack` → `artifacts/packages` |
| `publish-github-packages` | Push to `nuget.pkg.github.com` with `GITHUB_TOKEN` |
| `publish-nuget-org` | Push to nuget.org |
| `upload-release-packages` | `gh release upload` |
| `bump-build-version` / `commit-version-bump` | 4th segment bump after merge |
| `resolve-release-version` | Version from release tag |
| `raylib-build-test` / `raylib-pack-publish` | Raylib-specific layout |

Low-level `restore`, `build`, and `test` actions remain for custom callers; standard workflows use `dotnet-build` only.

## Repo layout (example `pull-request.yml`)

```yaml
name: Pull request
on:
  pull_request:
    branches: [main]
jobs:
  ci:
    uses: Novolis-Platform/novolis-workflows/.github/workflows/dotnet-pull-request.yml@main
```

Merge adds `permissions` and `skip_publish`. Release passes only `NUGET_API_KEY` (repo or org secret — no GitHub Environment required).

## Feeds

- **Merge** → `https://nuget.pkg.github.com/Novolis-Platform/index.json` (`packages: write`)
- **Release** → nuget.org + `.nupkg` on the GitHub Release

Cross-repo `Novolis.*` restore uses `GITHUB_TOKEN` (`packages: read` on PR jobs, `packages: write` on merge). Pass `packages_token` to `dotnet-build` / `raylib-build-test` only when a PAT with `read:packages` is required.

## Scaffold

```powershell
./novolis-governance/scripts/apply-pr-merge-release-workflows.ps1
```
