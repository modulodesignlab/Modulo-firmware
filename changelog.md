# Changelog

All notable changes to Modulo firmware will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

_No unreleased changes._

---

## [0.1.11] — 2026-06-10

### Base — v0.1.11

#### Fixed

- Bumped project version to coordinate with module update.

### Modules — v0.1.4

#### Fixed

- Removed `s_module_type` loading from NVS to prevent stale/incorrect module type reports.

---

## [0.1.10] — 2026-06-10

### Base — v0.1.10

#### Added

- Implemented CMD_GET_INFO (0x02) request in polling loop and post-OTA sequence to refresh module type and versions.

---

## [0.1.3] — 2026-06-10

### Modules — v0.1.3

#### Added

- Implemented CMD_GET_INFO (0x02) command to return unique chip ID, module type, and hardware/software versions.

---

## [0.1.9] — 2026-06-10

### Base — v0.1.9

#### Changed

- Increased CMD_FIRMWARE_UPDATE I2C response delay to 50ms to ensure slave NVS write completion.

---

## [0.1.2] — 2026-06-10

### Modules — v0.1.2

#### Changed

- Deferred NVS flash writes in CMD_FIRMWARE_UPDATE response to prevent blocking the I2C transaction.

---

## [0.1.8] — 2026-06-10

### Base — v0.1.8

#### Added

- Added support for asynchronous I2C module OTA progress and changelog rendering.
- Resolved I2C response length validation issues for module firmware update command.

#### Changed

- Display specific module type and version in web application.
- Changed default base IP to 192.168.1.178.

---

## [0.1.7] — 2026-06-10

### Base — v0.1.7

#### Added

- Added `CMD_GET_VBUS_VOLTAGE` I2C command to query VBUS voltage readings from connected modules.
- Implemented module-assisted USB-PD power negotiation when the Base's internal ADC is unavailable.

---

## [0.1.6] — 2026-06-10

### Base — v0.1.6

#### Changed

- Handled invalid VBUS ADC pin on Base gracefully to prevent initialization errors.
- Set default Base VBUS voltage reporting to 5V (5000 mV) when the internal ADC is not available.

---

## [0.1.5] — 2026-06-10

### Base — v0.1.5

#### Added

- Fixed HTTP client buffer overflow error during FOTA updates by increasing buffer size.
- Fixed front-end JavaScript console exception when WebSocket connection is closed.

---

## [0.1.4] — 2026-06-10

### Base — v0.1.4

#### Changed

- Incremented version to 0.1.4 for FOTA update flow verification.

---

## [0.1.3] — 2026-06-10

### Base — v0.1.3

#### Added

- Added real-time FOTA update progress tracking and detailed error reporting to the app dashboard.
- Configured asynchronous background task for OTA updates to avoid blocking WebSockets.
- Added default SSL CA certificate bundle (`esp_crt_bundle.h`) to verify HTTPS connections.

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
