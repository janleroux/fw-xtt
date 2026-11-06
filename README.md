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

Before publishing a real entry, define and validate its exact version, target hardware, immutable GitHub Release asset URL, file size, SHA-256 hash, required update format, and cross-device compatibility. The final published-entry schema is to be finalized with the OTA code and its tests; do not publish partly populated entries.

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
