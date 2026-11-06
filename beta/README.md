# XTT Early Access Testing Channel (`beta/`)

**Audience:** selected external XTT Early Access Testing Program participants.

Beta firmware must first pass the internal `dev/` acceptance checks. Testers provide real-world reliability findings and feedback; unresolved major issues prevent promotion to stable release.

The channel's update entry point is [`manifest.json`](manifest.json). Checks are initiated only by the user from the Coach Wi-Fi firmware-update menu; installations require confirmation.

**Current status:** no approved Coach or Sensor beta firmware is advertised.

A previously published `beta-version.txt` value (`0.1.0`) was legacy metadata with no verified Phase 1 firmware release asset. It has been retired so it cannot be mistaken for an available update.

Promotion to `release/` must point to **exactly the same qualified binary and SHA-256**; do not recompile on promotion.
