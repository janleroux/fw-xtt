# XTT Development Channel (`dev/`)

**Audience:** internal development hardware only.

This channel is intended for frequent Phase 1 firmware builds, diagnostics, fixes, and OTA reliability testing. A build here is not approved for early-access testers or general users.

The single authoritative update entry point is [`manifest.json`](manifest.json). The firmware check is **manual**, initiated from the Coach's Wi-Fi firmware-update menu; there is no background polling.

**Current status:** no Coach or Sensor firmware is qualified or advertised in the manifest.

### Historical note

The repository previously contained `dev/dev-version.txt` with version `0.1.1`, and a legacy GitHub Release tagged `xtt-dev` with `firmware.bin`. This is older experiment history, **not an approved current XTT Phase 1 firmware update**. The GitHub Release asset has been retained for traceability; the obsolete version pointer has been retired.

The firmware published here in future must meet the verification requirements described in the [repository README](../README.md).
