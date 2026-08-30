# XenoOne Releases

Public distribution boundary for immutable XenoOne release artifacts.

This repository intentionally contains **release artifacts and public trust metadata only**. Private Control/client source, private signing keys and publication credentials do not belong here.

## Component ownership

XenoOne currently publishes two independent release families with separate trust domains and separate discovery rules.

### XenoOne Control

Control V1 owns the repository-global GitHub **Latest** release because deployed Control instances discover:

`/releases/latest/download/xenoone-control-update.json`

Control tags use:

`v<major.minor.patch>`

A Control release publishes:

- `xenoone-control-<version>-linux-arm64.tar.gz` — immutable Raspberry Pi OS 64-bit ARM deployment archive;
- `xenoone-control-<version>-linux-arm64.tar.gz.sha256` — operator/audit checksum;
- `xenoone-control-update.json` — strict V1 signed stable-release metadata used by deployed Control discovery;
- `xenoone-control-update.json.sig` — raw Ed25519 signature over the exact V1 manifest bytes.

The onboard updater accepts only stable `major.minor.patch` releases, verifies the commissioned Control Ed25519 trust identity and archive SHA-256 before staging, and requires authoritative Control-side approval before installation.

**Do not publish another component as repository-global Latest while deployed Control V1 uses this discovery URL.**

### Windows Client

Windows client releases use a component-scoped namespace and do **not** become repository-global Latest.

Client tags use:

`client-windows-v<major.minor.patch>`

A Windows client release publishes:

- `xenoone-client-windows-<version>.zip` — immutable Windows application bundle;
- `xenoone-client-windows-<version>.zip.sha256` — operator/audit checksum;
- `xenoone-client-windows-update.json` — signed stable metadata;
- `xenoone-client-windows-update.json.sig` — raw Ed25519 signature over the canonical metadata decision payload.

Stable Windows discovery uses the component-specific pointer committed on `main`:

`client/windows/stable.json`

The client metadata authenticates the release schema, component, platform, version, channel, compatible `xenoone.v1` API range, immutable artifact URL, SHA-256, client release key ID and source commit. Windows client release metadata uses a dedicated Ed25519 identity that is distinct from the Control signing identity.

The client updater verifies the signed metadata **before** trusting version/API/artifact decisions, restricts artifacts to the `client-windows-v<version>` namespace, and verifies the downloaded ZIP SHA-256 before staging.

## Trust boundaries

- Control and client release signing identities are separate.
- A client release must never replace Control's repository-global Latest designation while Control V1 depends on it.
- Private signing keys and GitHub publication credentials never belong in this repository, distributed packages, Jira/Confluence, or target devices.
- Release assets are immutable. Existing release tags/assets are not overwritten.
- Source builds and `git pull` are not part of either production update path.

## Hardware validation

Release publication and hardware commissioning are separate evidence gates. Software release metadata may explicitly state that physical Pi/MCU validation is pending; absence of hardware evidence must never be represented as a passing hardware test.
