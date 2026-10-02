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
  <a href="https://novolis-platform.github.io/.github/novolis-workflows/"><img src="https://img.shields.io/badge/docs-portfolio-0a7ea3" alt="docs"/></a>
  <a href="https://github.com/Novolis-Platform/novolis-workflows/actions"><img src="https://img.shields.io/github/actions/workflow/status/Novolis-Platform/novolis-workflows/merge.yml?branch=main&label=merge&logo=github" alt="merge"/></a>
  <a href="https://github.com/orgs/Novolis-Platform/packages?repo_name=novolis-workflows"><img src="https://img.shields.io/badge/packages-GitHub%20Packages-0a7ea3?logo=nuget" alt="packages"/></a>
  <a href="https://github.com/Novolis-Platform"><img src="https://img.shields.io/badge/org-Novolis--Platform-111827" alt="org"/></a>
</p>

<p align="center">
  <a href="https://novolis-platform.github.io/.github/novolis-workflows/">Docs</a>
  ·
  <a href="https://nuget.pkg.github.com/Novolis-Platform/index.json"><code>https://nuget.pkg.github.com/Novolis-Platform/index.json</code></a>
  ·
  <a href="https://github.com/Novolis-Platform/.github/blob/main/profile/README.md">Org landing</a>
  ·
  <a href="https://github.com/Novolis-Platform/novolis-governance">Governance</a>
</p>

---
<!-- novolis-marketing:end -->
# novolis-workflows

Shared GitHub Actions for Novolis package and application repos.

## Version format

**`YEAR.MAJOR.MINOR.BUILD`** — four numeric segments only (no `-ci`, no `+metadata`).

Example: `2026.1.1.351` where `351` = `github.run_number`.

- Intent: `build/version.json` (`year`, `major`, `minor`)
- Same version on GitHub Packages and nuget.org

## Reusable workflows

| Workflow | Repo file | Purpose |
|----------|-----------|---------|
| `dotnet-pull-request.yml` | `pull-request.yml` | Restore, build, test |
| `dotnet-merge-publish.yml` | `merge.yml` | Build, pack, push to GitHub Packages and nuget.org |
| `dotnet-release-publish.yml` | `release.yml` | Pack, push to GitHub Packages, attach packages to the GitHub Release. Does not push nuget.org |
| `maui-pull-request.yml` | application PR workflow | Build .NET 10 MAUI Android + Windows and optional core tests |
| `maui-release.yml` | application release workflow | Produce signed Android APK + Windows MSIX, SHA256 sums, and attach to a GitHub release |

### MAUI release secrets

Callers of `maui-release.yml` provide these through `secrets: inherit` or an explicit mapping:

- `ANDROID_KEYSTORE_BASE64`
- `ANDROID_KEY_ALIAS`
- `ANDROID_KEYSTORE_PASSWORD`
- `ANDROID_KEY_PASSWORD`
- `WINDOWS_CERTIFICATE_BASE64`
- `WINDOWS_CERTIFICATE_PASSWORD`

The workflow deliberately does not generate ephemeral signing identities. Release artifacts keep stable application identities across versions.

## App workflows

`novolis-apps` calls these. The caller files only choose the trigger.

| Workflow | Caller |
|----------|--------|
| `apps-ci.yml` | Pull request and merge. `mode` is `pull_request`, `merge`, or `dispatch`. |
| `apps-release.yml` | Manual full release. No Play Store and no nuget.org. |
| `apps-play.yml` | Manual Play delivery from an existing tag. |

A failed check is named `channel / app`. The Result job lists every job and adds an annotation for each failure.

## Composite actions

| Action | Role |
|--------|------|
| `setup-dotnet` | Install .NET SDK |
| `authenticate-github-packages` | Add GPR source with `GITHUB_TOKEN` (or PAT) |
| `prepare-novolis-build` | Clone `novolis-governance`, set up .NET, authenticate GitHub Packages |
| `workflow-result` | Summary table of job results, with an annotation for each failure |
| `dispatch-org-landing` | Ask `Novolis-Platform/.github` to refresh the docs-home failure list |
| `checkout-sibling-repos` | Multi-repo checkout layout for CI |
| `read-version` | `YEAR.MAJOR.MINOR` from JSON + `BUILD` from `github.run_number` |
| `resolve-release-version` | Validate release tag against `version.json` |
| `dotnet-build` | Restore, build, optional test |
| `dotnet-pack-versioned` | `dotnet pack` with full four-part version |
| `publish-github-packages` / `publish-nuget-org` | Push artifacts |
| `upload-release-packages` | Attach `.nupkg`/`.snupkg` to an existing GitHub Release |
| `install-inno-setup` | Chocolatey install Inno Setup 6; output `iscc-path` |
| `write-sha256sums` | Write `SHA256SUMS.txt` for a file list |
| `ensure-github-release` | Create a GitHub Release with its assets in one publish, or upload onto a release that already exists |
| `publish-built-release` | Hash this run's installers, publish one GitHub Release, keep the newest tags |
| `upload-google-play-bundle` | Upload a signed Android App Bundle to a Google Play track through the Android Publisher API |

`upload-google-play-bundle` is a PowerShell composite action and does not
require Node, Python, or a third-party publishing runtime. Callers provide a
temporary service-account JSON path and select the Play track. Production
rollouts may set a fraction below `1`; internal, closed, and open tracks must
use a completed release.

