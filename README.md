# XTT Firmware Distribution

Public distribution repository for **XTT — X-Ten Trainer** firmware. Application source remains in the authorized `proj-xtt` Phase 1 projects (`xtt-coach-phase1-scope` and `xtt-sensor-phase1-scope`).

## Distribution channels

| Directory | Audience | Purpose |
| --- | --- | --- |
| [`dev/`](dev/) | Internal development hardware | Rapid developer-only builds and diagnostics; no stability guarantee |
| [`beta/`](beta/) | XTT Early Access Testing Program | Builds that passed internal checks, distributed to selected testers for feedback |
| [`release/`](release/) | General users | Qualified stable builds with no unresolved release-blocking defects |

The folders contain **channel manifests**, not firmware binaries. Firmware binaries belong in **GitHub Release assets**. Both Coach and Sensor may have independent versions and update availability.

## Update policy

1. The XTT-Coach stores a selected channel (dev, beta, or release).
2. **No automatic update checking** at startup, in the background, or on a timer. The user opens the Coach's Wi-Fi firmware-update menu and selects **Check for Updates**.
3. Only then does the Coach connect to configured Wi-Fi, fetch the selected channel's `manifest.json`, validate the response, and show the available firmware and versions.
4. Installation requires a deliberate user confirmation. The Coach downloads and verifies approved firmware for itself and, when supported, stages a compatible Sensor update over BLE DFU.
5. XTT-cloud may **notify** users of new versions in the future; that does not authorize automatic download or installation.

These steps define the intended behavior; OTA installation has **not** yet been implemented or qualified for the Phase 1 firmware.

## Manifests (schema v1, bootstrap)

Each channel has a JSON document:

```json
{
  "schema_version": 1,
  "channel": "dev",
  "coach": null,
  "sensor": null
}
```

`null` means **no approved OTA firmware is published for that device on this channel**. All three manifests intentionally start with both devices set to `null`. The update client must treat missing, invalid, or unpublished entries as **no update**, never as permission to download an arbitrary asset.

The Coach OTA1 implementation now recognises this exact published \`coach\` entry shape, while leaving \`sensor\` at \`null\` until Sensor OTA is implemented:

\`\`\`json
{
  "schema_version": 1,
  "channel": "dev",
  "coach": {
    "version": "0.1.1",
    "hardware": "seeed-xiao-esp32-s3-plus",
    "format": "esp32-app-bin",
    "url": "https://github.com/janleroux/fw-xtt/releases/download/coach-0.1.1/xtt-coach-0.1.1.bin",
    "size_bytes": 1234567,
    "sha256": "REPLACE_WITH_REAL_64_CHARACTER_SHA256_CHECKSUM",
    "source_commit": "REPLACE_WITH_ACTUAL_40_CHARACTER_SOURCE_SHA"
  },
  "sensor": null
}
\`\`\`

**Example only; intentionally invalid:** the size, SHA-256 and source commit above are placeholders, not an approved package. Never copy it into a live channel manifest. Use a numeric, increasing \`major.minor.patch\` version; the first OTA1 Coach build identifies itself as \`0.1.0\`, so the first normal update offer must be a higher version. The URL must point to an immutable named \`.bin\` GitHub Release asset in this repository (no moving \`latest\` pointer). The firmware file must fit the actual Coach OTA slot. Only populate \`coach\` after a compiled binary, hash, target and wired recovery have been validated. Passing a SHA-256 hash does not by itself prove that the firmware is correctly built for this hardware.

The Coach downloads the channel manifest over certificate-validated HTTPS on an explicit user request, rejects malformed/unpublished entries and wrong target or outdated version, then requires a separate user install gesture. After streaming the complete image to the inactive ESP32 OTA partition, it checks SHA-256 before selecting the new image and restarting. **This is implementation intent and source behaviour, not a claim of hardware acceptance.** There is no unattended checking or upgrading.

## Release and promotion rules

1. Build from an identified source commit in the two authorized firmware projects. Record Coach and Sensor versions independently.
2. Verify the binary's hardware target, actual flash size, update compatibility, SHA-256 checksum, and recovery process before enabling any channel entry.
3. Store firmware in a versioned GitHub Release using **immutable build-specific asset names/URLs**, not a moving `latest` URL. Preserve the compiled bytes and their hashes.
4. Publish first to `dev`. Move to `beta` only after internal acceptance checks and to `release` only after beta acceptance.
5. **Promote the identical tested binary** by changing the destination channel's manifest pointer; do not rebuild it just to promote it. A firmware version identifies a build, not its distribution channel.
6. When a defect requires new code, create a new build/version and qualify it through the channels again.
7. Keep the previous known-good firmware and a wired recovery procedure available throughout development. Test interruption, wrong-target rejection, checksum failure, and Coach/Sensor version compatibility before tester rollout.

## Current state

**No firmware is presently published through these manifests.** This is intentional: the existing historical GitHub Releases were created under an older scheme and have not been qualified as Phase 1 OTA packages. Old GitHub Release tags/assets remain available for traceability but are not active channel pointers.

The legacy `*-version.txt` pointers and the legacy `official/` folder have been retired in favor of explicit `dev/`, `beta/`, and `release/` manifests.
