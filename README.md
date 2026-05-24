# novolis-workflows

Reusable GitHub Actions workflows for Novolis repositories.

## Publishing

Packages are published to **[GitHub Packages](https://github.com/orgs/Novolis-Platform/packages)** (NuGet feed), **not** nuget.org.

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| `dotnet-merge-publish.yml` | `merge.yml` on push to `main` | Build, test, pack, push to GitHub Packages, bump build |
| `dotnet-pull-request.yml` | PR | Build and test only |
| `dotnet-publish-nuget.yml` | `release.yml` | Pack and push to GitHub Packages |

Feed URL (org):

```text
https://nuget.pkg.github.com/Novolis-Platform/index.json
```

## Repo setup

1. `.novolis/version.props` starting at `0.0.1.1`
2. `Directory.Build.targets` importing `novolis-governance/build/Novolis.Version.targets`
3. `merge.yml` with `permissions: packages: write`

Run `novolis-governance/scripts/configure-package-publishing.ps1` to scaffold a repo.
