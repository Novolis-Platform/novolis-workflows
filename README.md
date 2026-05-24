# novolis-workflows

Shared GitHub Actions for Novolis package repos. Repo workflows are thin triggers; logic lives here.

## Reusable workflows

| Workflow | Repo file | Purpose |
|----------|-----------|---------|
| `dotnet-pull-request.yml` | `pull-request.yml` | Restore, build, test |
| `dotnet-merge-publish.yml` | `merge.yml` | Above + pack (`2026.1.0-ci.{run}`) + GitHub Packages |
| `dotnet-release-publish.yml` | `release.yml` | Pack at release tag (`2026.1.0`), nuget.org, attach assets |

## Versioning

- Human intent: `build/version.json` per repo (`sdkYear`, `apiBreak`, `feature`).
- CI build number: `github.run_number` only (not committed).
- Internal GPR: `2026.1.0-ci.382`
- Stable nuget.org: `2026.1.0` (tag `v2026.1.0` must match `version.json`).

## Composite actions

| Action | Role |
|--------|------|
| `read-version` | Parse `build/version.json`, compute stable/ci versions and assembly metadata |
| `dotnet-pack-versioned` | `dotnet pack` with explicit version properties |
| `dotnet-build` | SDK setup, optional GPR auth, restore/build/test |
| `pack` | Delegates to `read-version` + `dotnet-pack-versioned` |
| `publish-github-packages` | Push to `nuget.pkg.github.com` with `GITHUB_TOKEN` |
| `publish-nuget-org` | Push to nuget.org |
| `resolve-release-version` | Validate release tag against `build/version.json` |

Legacy (unused by merge): `bump-build-version`, `commit-version-bump`.
