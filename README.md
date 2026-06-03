# Modulo Firmware

**Official firmware distribution repository for the Modulo desktop ecosystem.**

> ⚠️ **This repository does not contain source code.**
> Firmware source code is maintained in a separate private repository. This repository is used exclusively for OTA firmware distribution, manifests, release notes, changelogs, and public firmware metadata.

---

## About Modulo

Modulo is a modular desktop ecosystem composed of a central **Base** and interchangeable **Smart Modules**. The Base serves as the foundation of the system, providing power distribution, a communication bus, Wi-Fi connectivity, firmware update management, and mobile app integration.

Users can insert and remove modules at any time. Each module contains its own microcontroller and firmware, enabling independent operation and seamless hot-swapping.

### Supported Modules

| Module | Description |
|---|---|
| **Bluetooth Speaker** | Wireless audio playback module |
| **Environmental Monitor** | Temperature, humidity, and air quality sensing module |
| **1.28" Smart Screen** | Compact display module for notifications and widgets |
| **USB-C Device Charger** | USB-C power delivery module for device charging |
| **Wireless Charger** | Qi-compatible wireless charging module |
| **LED Tower** | Programmable RGB LED lighting module |

The **Base** unit also runs its own firmware and manages the lifecycle of all connected modules.

---

## Purpose of This Repository

This repository serves as the single source of truth for Modulo firmware distribution. It provides:

- **OTA Manifests** — Machine-readable firmware metadata used by Modulo devices to discover and download updates.
- **Firmware Binaries** — Distributed as assets attached to [GitHub Releases](../../releases).
- **Release Notes** — Human-readable descriptions of what changed in each firmware version.
- **Changelogs** — A consolidated history of all firmware changes across the ecosystem.

---

## OTA Update Workflow

Modulo devices receive firmware updates over the air (OTA) through the following process:

```
┌──────────────┐         ┌──────────────────┐         ┌────────────────┐
│  Modulo Base  │───────▶│  manifest.json    │───────▶│  GitHub Release │
│  checks for   │        │  (this repo)      │        │  (binary asset) │
│  updates       │        │                    │        │                  │
└──────────────┘         └──────────────────┘         └────────────────┘
```

1. The Modulo Base periodically fetches `manifest.json` from this repository.
2. It compares the manifest version against each module's currently installed firmware version.
3. If a newer version is available, the binary is downloaded from the URL specified in the manifest.
4. The downloaded binary is validated against the SHA-256 checksum in the manifest.
5. Upon successful validation, the firmware is applied to the target device.

For a detailed technical overview, see [OTA Overview](docs/ota-overview.md).

---

## Repository Structure

```
modulo-firmware/
├── README.md                   # This file
├── LICENSE.md                  # Proprietary license
├── manifest.json               # OTA firmware manifest
├── changelog.md                # Consolidated changelog
├── docs/
│   ├── ota-overview.md         # OTA architecture documentation
│   └── firmware-versioning.md  # Versioning strategy documentation
└── .github/
    └── ISSUE_TEMPLATE/
        └── firmware-bug-report.md  # Bug report template
```

---

## Versioning

Modulo firmware follows [Semantic Versioning](https://semver.org/) (`MAJOR.MINOR.PATCH`). Each module is versioned independently. Module firmware may declare a `minimum_base_version` to ensure compatibility with the Base.

For details, see [Firmware Versioning](docs/firmware-versioning.md).

---

## Reporting Issues

If you encounter a firmware-related issue, please [open an issue](../../issues/new/choose) using the provided bug report template. Include your device type, firmware version, and steps to reproduce the problem.

---

## License

All firmware distributed through this repository is proprietary software. See [LICENSE.md](LICENSE.md) for full terms.

**Redistribution, modification, and reverse engineering are strictly prohibited.**

---

<p align="center">
  Copyright &copy; 2025–2026 Modulo Design Lab. All rights reserved.
</p>
