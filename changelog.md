# Changelog

All notable changes to Modulo firmware will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

_No unreleased changes._

---

## [0.1.35] — 2026-06-15

### Base — v0.1.35

#### Fixed

- Implemented dynamic I2C OTA phase-shift recovery: detects if the Slave sends a stale/misaligned response chunk index (due to a previous timeout) and performs a clean read transaction without transmitting, clearing the stale queue and restoring proper frame alignment.
- Allowed `i2c_manager_send_frame` to perform read-only transactions (when `tx` is NULL) on both standard and no-offset addresses.

---

## [0.1.34] — 2026-06-15

### Base — v0.1.34

#### Fixed

- Refined I2C read length behavior: allowed reading extra bytes (`rx_len + 48`) during non-data commands (like `SUB_OTA_BEGIN` and `SUB_OTA_END`) to clear any stale frames (e.g. legacy `CMD_PING` responses) from the Slave's TX FIFO, while maintaining strict `rx_len` reads during `SUB_OTA_DATA` to prevent Slave FIFO underflows.

---

## [0.1.33] — 2026-06-15

### Base — v0.1.33

#### Changed

- Added compatibility delay (220ms wait) for older Slave modules (versions below 20/0.1.10) to accommodate slower flash writes and LED refreshes during I2C streaming updates.

---

## [0.1.32] — 2026-06-15

### Base — v0.1.32

#### Added

- Implemented I2C polling optimizations and WebSocket message handling for Bluetooth Speaker controls.

---

## [0.1.31] — 2026-06-15

### Base — v0.1.31

#### Changed

- Minor internal updates.

---

## [0.1.20] — 2026-06-12

### Base — v0.1.20

#### Changed

- Increased default I2C command response wait time to 50ms for improved communication stability.

---

## [0.1.19] — 2026-06-12

### Base — v0.1.19

#### Fixed

- Fixed I2C OTA delay logic for legacy slave firmware.

---

## [0.1.18] — 2026-06-12

### Base — v0.1.18

#### Added

- Implemented dynamic delay compatibility for slave modules running v0.1.6.

---

## [0.1.17] — 2026-06-12

### Base — v0.1.17

#### Changed

- Optimized Base OTA_BEGIN wait time to 5000ms.

---

## [0.1.16] — 2026-06-12

### Base — v0.1.16

#### Fixed

- Fixed I2C OTA Chunk 0 timeout: removed slave 200ms delay and increased master wait to 30ms.

---

## [0.1.15] — 2026-06-12

### Base — v0.1.15

#### Fixed

- Increased HTTP client buffer size to 4096 bytes to support long redirect URLs from GitHub releases to AWS S3.
- Implemented HTTP client redirection handling (up to 5 redirects).

---

## [0.1.14] — 2026-06-12

### Base — v0.1.14

#### Changed

- Preparatory updates for HTTP client redirection.

---

## [0.1.13] — 2026-06-12

### Base — v0.1.13

#### Added

- Initial release containing I2C polling pause during Slave OTA updates and orange blinking LED support.

---

## [0.1.12] — 2026-06-10

### Base — v0.1.12

#### Added

- Implemented real I2C OTA firmware streaming protocol over I2C (BEGIN/DATA/END sub-commands).
- Configured dynamic I2C response wait times depending on the specific sub-command.
- Increased `module_ota_task` stack size to support full HTTPS download and streaming.

### Modules — v0.1.5

#### Added

- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.
- Switched partition table layout to two-OTA partitions.

---

## [0.1.11] — 2026-06-15

### Bluetooth Speaker — v0.1.11

#### Fixed

- Implemented automatic I2C driver recovery to clear hardware bus lockups/hangs: distinguished driver/communication errors (negative return values) from standard idle timeouts. When an error is detected, the Slave automatically de-initializes and re-initializes its I2C driver to reset the hardware peripheral and release the SDA/SCL lines.

### Base — v0.1.11 (Historical)

#### Fixed

- Bumped project version to coordinate with module update.

### Modules — v0.1.4 (Historical)

#### Fixed

- Removed `s_module_type` loading from NVS to prevent stale/incorrect module type reports.

---

## [0.1.10] — 2026-06-15

### Base — v0.1.10

#### Added

- Implemented CMD_GET_INFO (0x02) request in polling loop and post-OTA sequence to refresh module type and versions.

### Bluetooth Speaker — v0.1.10

#### Added

- Implemented Bluetooth volume control (0-100), song titles, progress tracking, and listening states.
- Optimized I2C OTA updates by throttling LED status strip refresh rate (once every 500ms) to eliminate high latency and avoid blocking I2C transactions.

---

## [0.1.9] — 2026-06-15

### Base — v0.1.9

#### Changed

- Increased CMD_FIRMWARE_UPDATE I2C response delay to 50ms to ensure slave NVS write completion.

### Bluetooth Speaker — v0.1.9

#### Added

- Skeletons and commands for bluetooth controls (play, pause, next, prev, volume up/down).

---

## [0.1.8] — 2026-06-10

### Base — v0.1.8

#### Added

- Added support for asynchronous I2C module OTA progress and changelog rendering.
- Resolved I2C response length validation issues for module firmware update command.

#### Changed

- Display specific module type and version in web application.
- Changed default base IP to 192.168.1.178.

### Bluetooth Speaker — v0.1.8

#### Added

- Skeletons for bluetooth controls.

---

## [0.1.7] — 2026-06-10

### Bluetooth Speaker — v0.1.7

#### Added

- Skeletons and build configurations.

---

## [0.1.3] — 2026-06-10

### Modules — v0.1.3

#### Added

- Implemented CMD_GET_INFO (0x02) command to return unique chip ID, module type, and hardware/software versions.

---

## [0.1.2] — 2026-06-10

### Modules — v0.1.2

#### Changed

- Deferred NVS flash writes in CMD_FIRMWARE_UPDATE response to prevent blocking the I2C transaction.

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
