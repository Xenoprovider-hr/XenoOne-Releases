# XenoOne Releases

Public distribution channel for immutable XenoOne release artifacts.

This repository intentionally does **not** contain the private Control source tree. Stable GitHub Releases published here contain prebuilt Raspberry Pi OS 64-bit ARM artifacts plus signed update metadata consumed by deployed XenoOne Control systems.

## Release assets

A Control release publishes:

- `xenoone-control-<version>-linux-arm64.tar.gz` — immutable ARM64 deployment archive;
- `xenoone-control-<version>-linux-arm64.tar.gz.sha256` — operator/audit checksum;
- `xenoone-control-update.json` — signed stable-release metadata used for discovery;
- `xenoone-control-update.json.sig` — raw Ed25519 signature over the exact manifest bytes.

The onboard updater accepts only stable `major.minor.patch` releases, verifies the pinned Ed25519 signature and archive SHA-256 before staging, and requires explicit Captain approval before installation.

Source builds and `git pull` are not part of the production update path.
