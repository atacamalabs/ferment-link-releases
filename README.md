# FERMENT Link Releases

Public releases-only mirror for [FERMENT Link](https://github.com/atacamalabs/ferment-link) firmware (formerly "Project PLATO"). This repo contains no source code — only compiled `.bin` releases and the `manifest.json` that FERMENT Link devices check over-the-air for updates.

Kept public and separate from the private source repo so devices can fetch `manifest.json` and release binaries via plain unauthenticated HTTP (`raw.githubusercontent.com`), without needing a GitHub account/token baked into the firmware.

## How a release gets published here

### Headless/Ethernet hub (`firmware/FERMENT_Link/`)

1. In the main `ferment-link` repo: bump `FIRMWARE_VERSION` in `firmware/FERMENT_Link/config.h`, commit, tag `vX.Y.Z`.
2. Arduino IDE: **Sketch → Export Compiled Binary** to produce `FERMENT_Link.ino.bin`.
3. Here: create a GitHub Release tagged `vX.Y.Z`, attach `FERMENT_Link.ino.bin`.
4. Here: update `manifest.json` with the new version and download URL, commit, push.

Devices poll `https://raw.githubusercontent.com/atacamalabs/ferment-link-releases/main/manifest.json` (via `POST /api/ota/check`) and compare against their own running version.

### Touchscreen tier (`firmware/FERMENT_Link_ESP-IDF/`)

The touchscreen board's `.bin` isn't interchangeable with the headless hub's (different partition scheme, different peripherals, entirely different build system) - it gets its **own** manifest and its own release tags, distinguished with a `touchscreen-` prefix so they never collide with the headless build's `vX.Y.Z` tags even when both share the same `FIRMWARE_VERSION`.

1. In the main `ferment-link` repo: bump `FIRMWARE_VERSION` in `firmware/FERMENT_Link/config.h` (shared by both tiers), commit, tag `vX.Y.Z`.
2. `idf.py build` in `firmware/FERMENT_Link_ESP-IDF/` to produce `build/main.bin`. Only the app image is needed here, not the bootloader or partition table - `esp_https_ota()` only ever replaces the currently-inactive `ota_0`/`ota_1` app partition.
3. Here: create a GitHub Release tagged `touchscreen-vX.Y.Z`, attach `main.bin` (renamed to something identifiable, e.g. `FERMENT_Link_Touchscreen_vX.Y.Z.bin`).
4. Here: update `manifest-touchscreen.json` with the new version and download URL, commit, push.

Touchscreen devices poll `https://raw.githubusercontent.com/atacamalabs/ferment-link-releases/main/manifest-touchscreen.json` instead of `manifest.json`.
