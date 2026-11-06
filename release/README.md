# XTT Stable Release Channel (`release/`)

**Audience:** general XTT users.

This channel is reserved for firmware that has completed internal and beta qualification, passed the agreed Phase 1 readiness tests, and has no unresolved release-blocking defects.

The channel's update entry point is [`manifest.json`](manifest.json). The Coach checks only when the user requests it through the Wi-Fi firmware-update menu.

**Current status:** no qualified stable Coach or Sensor firmware is advertised.

This directory supersedes the legacy `official/` naming. The old `official-version.txt` value (`1.0`) had no verified stable OTA firmware asset and must not be interpreted as a qualified release.

Stable promotion reuses the byte-identical tested beta asset and checksum, with independently versioned Coach and Sensor firmware.
