# laravel-moat AUR

Arch Linux AUR package for [laravel/moat](https://github.com/laravel/moat) -- a CLI tool that reviews the security posture of your GitHub organization and repositories.

This package installs the prebuilt binary from upstream releases. No Rust toolchain required.

This repository automatically stays in sync with upstream releases via a GitHub Actions workflow.

## Usage

```bash
# Install with an AUR helper
yay -S laravel-moat
# or
paru -S laravel-moat

# Run moat
moat <organization>
```

## Repository layout

```
PKGBUILD                         Arch Linux package build definition
.SRCINFO                         AUR package metadata (auto-generated)
.github/workflows/sync-aur.yml   Workflow that polls for upstream releases and pushes to the AUR
```

## How it works

Every hour (and on manual `workflow_dispatch`), the sync workflow:

1. Checks the latest GitHub release from `laravel/moat` via the API
2. Reads the current version from `PKGBUILD`
3. If a new release is found, extracts the SHA-256 checksums from the release API for both `x86_64` and `aarch64` binaries, updates `PKGBUILD`, regenerates `.SRCINFO` via `makepkg --printsrcinfo`, and pushes to `aur@aur.archlinux.org:laravel-moat.git`

Checksums are taken from the release API's `digest` field rather than re-downloading the tarballs, making the workflow fast and lightweight.


