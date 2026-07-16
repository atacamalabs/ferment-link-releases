# FERMENT Link Releases

Public releases-only mirror for [FERMENT Link](https://github.com/atacamalabs/ferment-link) firmware (formerly "Project PLATO"). This repo contains no source code — only compiled `.bin` releases and the manifest files that FERMENT Link devices check over-the-air for updates.

Kept public and separate from the private source repo so devices can fetch manifests and release binaries via plain unauthenticated HTTP (`raw.githubusercontent.com`), without needing a GitHub account/token baked into the firmware.

## Current platform: ESP32-P4/C6 touchscreen (16 July 2026)

FERMENT Link committed to the ESP32-P4/C6 platform on 16 July 2026 (Waveshare ESP32-P4-WIFI6-Touch-LCD-4B). This is the sole ongoing firmware target - both prior tiers below are discontinued/archived.

### How a release gets published (`firmware/FERMENT_Link_ESP-IDF_P4/`)

1. In the main `ferment-link` repo: bump `FIRMWARE_VERSION` in `firmware/FERMENT_Link_ESP-IDF_P4/main/config.h`, commit, tag `p4-touchscreen-vX.Y.Z`.
2. `idf.py build` in `firmware/FERMENT_Link_ESP-IDF_P4/` to produce `build/ferment_link_p4.bin`. Only the app image is needed, not the bootloader or partition table - `esp_https_ota()` only ever replaces the currently-inactive `ota_0`/`ota_1` app partition.
3. Here: create a GitHub Release tagged `p4-touchscreen-vX.Y.Z`, attach the `.bin` (renamed to something identifiable, e.g. `FERMENT_Link_P4_vX.Y.Z.bin`).
4. Here: update `manifest-p4.json` with the new version and download URL, commit, push.

P4 devices poll `https://raw.githubusercontent.com/atacamalabs/ferment-link-releases/main/manifest-p4.json` (via `POST /api/ota/check` or the on-device Firmware screen) and compare against their own running version.

---

## Legacy platforms (discontinued/archived)

### Headless/Ethernet hub (`firmware/FERMENT_Link/`) - retired 13 July 2026

The Arduino headless build was deleted from the source repo; `manifest.json` is left here for historical reference only, not actively updated.

1. In the main `ferment-link` repo: bump `FIRMWARE_VERSION` in `firmware/FERMENT_Link/config.h`, commit, tag `vX.Y.Z`.
2. Arduino IDE: **Sketch → Export Compiled Binary** to produce `FERMENT_Link.ino.bin`.
3. Here: create a GitHub Release tagged `vX.Y.Z`, attach `FERMENT_Link.ino.bin`.
4. Here: update `manifest.json` with the new version and download URL, commit, push.

### ESP32-S3 touchscreen tier (`firmware/FERMENT_Link_ESP-IDF/`) - discontinued 16 July 2026

Superseded by the P4/C6 platform above. `manifest-touchscreen.json` is left here for historical reference only, not actively updated.

1. In the main `ferment-link` repo: bump `FIRMWARE_VERSION` in `firmware/FERMENT_Link_ESP-IDF/main/config.h`, commit, tag `touchscreen-vX.Y.Z`.
2. `idf.py build` in `firmware/FERMENT_Link_ESP-IDF/` to produce `build/main.bin`.
3. Here: create a GitHub Release tagged `touchscreen-vX.Y.Z`, attach `main.bin` (renamed, e.g. `FERMENT_Link_Touchscreen_vX.Y.Z.bin`).
4. Here: update `manifest-touchscreen.json` with the new version and download URL, commit, push.
