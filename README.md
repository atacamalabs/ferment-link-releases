# FERMENT Link Releases

Public releases-only mirror for [FERMENT Link](https://github.com/atacamalabs/ferment-link) firmware (formerly "Project PLATO"). This repo contains no source code — only compiled `.bin` releases and the `manifest.json` that FERMENT Link devices check over-the-air for updates.

Kept public and separate from the private source repo so devices can fetch `manifest.json` and release binaries via plain unauthenticated HTTP (`raw.githubusercontent.com`), without needing a GitHub account/token baked into the firmware.

## How a release gets published here

1. In the main `ferment-link` repo: bump `FIRMWARE_VERSION` in `firmware/FERMENT_Link/config.h`, commit, tag `vX.Y.Z`.
2. Arduino IDE: **Sketch → Export Compiled Binary** to produce `FERMENT_Link.ino.bin`.
3. Here: create a GitHub Release tagged `vX.Y.Z`, attach `FERMENT_Link.ino.bin`.
4. Here: update `manifest.json` with the new version and download URL, commit, push.

Devices poll `https://raw.githubusercontent.com/atacamalabs/ferment-link-releases/main/manifest.json` (via `POST /api/ota/check`) and compare against their own running version.
