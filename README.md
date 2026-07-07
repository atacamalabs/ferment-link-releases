# PLATO Releases

Public releases-only mirror for [Project PLATO](https://github.com/atacamalabs/plato) firmware. This repo contains no source code — only compiled `.bin` releases and the `manifest.json` that PLATO devices check over-the-air for updates.

Kept public and separate from the private source repo so devices can fetch `manifest.json` and release binaries via plain unauthenticated HTTP (`raw.githubusercontent.com`), without needing a GitHub account/token baked into the firmware.

## How a release gets published here

1. In the main `plato` repo: bump `FIRMWARE_VERSION` in `firmware/PLATO_V1/config.h`, commit, tag `vX.Y.Z`.
2. Arduino IDE: **Sketch → Export Compiled Binary** to produce `PLATO_V1.ino.bin`.
3. Here: create a GitHub Release tagged `vX.Y.Z`, attach `PLATO_V1.ino.bin`.
4. Here: update `manifest.json` with the new version and download URL, commit, push.

Devices poll `https://raw.githubusercontent.com/atacamalabs/plato-releases/main/manifest.json` (via `POST /api/ota/check`) and compare against their own running version.
