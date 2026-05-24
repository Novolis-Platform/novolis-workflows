# novolis-workflows

Shared GitHub Actions for Novolis package repos.

## Version format

**`YEAR.MAJOR.MINOR.BUILD`** — four numeric segments only (no `-ci`, no `+metadata`).

Example: `2026.1.1.351` where `351` = `github.run_number`.

- Intent: `build/version.json` (`year`, `major`, `minor`)
- Same version on GitHub Packages and nuget.org

## Reusable workflows

| Workflow | Repo file | Purpose |
|----------|-----------|---------|
| `dotnet-pull-request.yml` | `pull-request.yml` | Restore, build, test |
| `dotnet-merge-publish.yml` | `merge.yml` | Build, pack, push to GitHub Packages |
| `dotnet-release-publish.yml` | `release.yml` | Pack, push to nuget.org |

## Composite actions

| Action | Role |
|--------|------|
| `read-version` | `YEAR.MAJOR.MINOR` from JSON + `BUILD` from `github.run_number` |
| `dotnet-pack-versioned` | `dotnet pack` with full four-part version |
| `dotnet-build` | Restore, build, optional test |
| `publish-github-packages` / `publish-nuget-org` | Push artifacts |
