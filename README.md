# novolis-workflows

Central reusable GitHub workflows and composite actions for Novolis repositories.

## Workflows

| Workflow | Use in repo |
|----------|-------------|
| `dotnet-ci.yml` | Generic CI (build + test) |
| `dotnet-pull-request.yml` | PR validation |
| `dotnet-merge-publish.yml` | Main branch: build, test, pack, publish, bump `.novolis/version.props` build |
| `dotnet-publish-nuget.yml` | Tag / GitHub Release publish |
| `dotnet-pack.yml` | Pack artifacts only |

## Repo setup

1. Add `.novolis/version.props` starting at `0.0.1.1` (see `novolis-governance/build/Novolis.Version.props`).
2. Import version props in `Directory.Build.props` and `Novolis.Version.targets` in `Directory.Build.targets`.
3. Add `.github/workflows/merge.yml` and `pull-request.yml` (run `novolis-governance/scripts/configure-package-publishing.ps1`).

## Versioning

4-part semver: `major.minor.patch.build`. The **build** segment increments automatically on each successful `merge.yml` run.
