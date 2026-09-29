# Release

This repository publishes with the org CalVer scheme (`2026.1.*`) via `merge.yml` to GitHub Packages.

See [release-policy](https://github.com/Novolis-Platform/novolis-governance/blob/main/docs/release-policy.md).

The shared `upload-google-play-bundle` composite action performs the Google
Android Publisher API edit/upload/track/commit sequence for signed AAB files.
Product repositories should keep their Play workflow and app catalog metadata
in the product repository while consuming this action from `main`.

## Packages

- (no packable ``Novolis.*`` projects detected — see repository README)

## Consumers

Restore from nuget.org + `https://nuget.pkg.github.com/Novolis-Platform/index.json` only.
