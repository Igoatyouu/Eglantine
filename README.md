# Eglantine macOS builds

This fork of [Djihads80/Eglantine](https://github.com/Djihads80/Eglantine) builds a macOS (Apple Silicon) app of every upstream release.

- `nx2` mirrors upstream's `nx2`, and the tags mirror upstream's tags.
- `macos-ci` (this branch, the default one) holds only the workflow and the patches.
- [`.github/workflows/macos-release.yml`](.github/workflows/macos-release.yml) runs every 30 minutes: it syncs `nx2` and the tags, then, when upstream has a release this repository does not, builds that release with the patches in [`patches/`](patches) and publishes the app (`.zip` and `.dmg`) as a release under the same tag. Run it by hand from the Actions tab to build a given tag again.

## Setup

Mirroring `nx2` needs a `SYNC_TOKEN` secret: GitHub's own workflow token cannot push upstream commits that change `.github/workflows`. Create a fine-grained token for this repository only, with **Contents: Read and write** and **Workflows: Read and write**, and add it under Settings → Secrets and variables → Actions. Without it the releases are still built; only the mirror stops.

Upstream's own workflows are disabled here, so a sync does not start their full build matrix.

## Patches

- `0001-host-shaders-quote-include-path.patch`: the shader include paths are quoted as whole arguments, so a checkout whose path holds a space builds.

A patch upstream already has is skipped, and one that no longer applies is skipped with a warning in the run.

## Install

Unzip `eglantine-macos-arm64.zip` (or open the `.dmg`) and move `eglantine.app` to Applications. The app is not notarized: right-click it and choose Open the first time, or run `xattr -dr com.apple.quarantine /Applications/eglantine.app`.
