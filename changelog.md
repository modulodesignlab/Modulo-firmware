# Changelog

All notable changes to Modulo firmware will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

_No unreleased changes._

---

## [0.1.2] — 2026-06-10

### Base — v0.1.2

#### Added

- Added VBUS voltage monitoring on serial output every 10 seconds.

---

## [0.1.1] — 2026-06-03

### Changed

- Bumped software version of Modulo Base (`base_app`) and Modulo Slave (`slave_app`) to `0.1.1`.
- Updated version display and formatting in mobile app dashboard and update manager to correctly display semantic versions.

---

## [0.1.0] — 2026-06-03

### Base — v0.1.0

#### Added

- First functional firmware release for the Modulo Base.
- Wi-Fi provisioning via BLE (NimBLE).
- OTA firmware update support with dual OTA partitions and rollback.
- WebSocket-based communication with the mobile app.
- Module hot-swap detection and lifecycle management.
- Power distribution and communication bus management.

## [0.0.1] — 2026-06-03

### Added

- Initial repository setup.
- Created OTA firmware manifest (`manifest.json`) with schema version 1.
- Added placeholder firmware entries for all supported devices:
  - Base
  - Bluetooth Speaker
  - Environmental Monitor
  - 1.28" Smart Screen
  - USB-C Device Charger
  - Wireless Charger
  - LED Tower
- Added OTA architecture documentation (`docs/ota-overview.md`).
- Added firmware versioning documentation (`docs/firmware-versioning.md`).
- Added GitHub issue template for firmware bug reports.
- Added proprietary license (`LICENSE.md`).
