<!-- novolis-marketing:start -->
<p align="center">
  <a href="https://github.com/Novolis-Platform">
    <img src="https://raw.githubusercontent.com/Novolis-Platform/.github/main/brand/logo-brand-transparent.svg" width="360" alt="Novolis"/>
  </a>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/Novolis-Platform/.github/main/brand/banners/novolis-workflows.svg" width="100%" alt="novolis-workflows"/>
</p>

<p align="center">
  <strong>Reusable GitHub Actions</strong><br/>
  Reusable CI workflows for build, pack, and release across Novolis repos.
</p>

<p align="center">
  <a href="https://github.com/Novolis-Platform/novolis-workflows/actions"><img src="https://img.shields.io/github/actions/workflow/status/Novolis-Platform/novolis-workflows/merge.yml?branch=main&label=merge&logo=github" alt="merge"/></a>
  <a href="https://github.com/orgs/Novolis-Platform/packages?repo_name=novolis-workflows"><img src="https://img.shields.io/badge/packages-GitHub%20Packages-0a7ea3?logo=nuget" alt="packages"/></a>
  <a href="https://github.com/Novolis-Platform"><img src="https://img.shields.io/badge/org-Novolis--Platform-111827" alt="org"/></a>
</p>

<p align="center">
  <a href="https://nuget.pkg.github.com/Novolis-Platform/index.json"><code>https://nuget.pkg.github.com/Novolis-Platform/index.json</code></a>
  ·
  <a href="https://github.com/Novolis-Platform/.github/blob/main/profile/README.md">Org landing</a>
  ·
  <a href="https://github.com/Novolis-Platform/novolis-governance">Governance</a>
</p>

---
<!-- novolis-marketing:end -->
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

