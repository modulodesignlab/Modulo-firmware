# Changelog

All notable changes to Modulo firmware will be documented in this file, structured by software component.

---

## Modulo Base

### [0.1.50] — 2026-07-07
- Added support for reading and forwarding I2C and EPD error telemetry in cJSON WebSocket packet.

### [0.1.49] — 2026-07-07
- Added AQI (Air Quality Index) field serialization to environmental sensor WebSocket JSON.

### [0.1.48] — 2026-07-07
- Added I2C polling support for Environmental Monitor module type (0x05).
- Implemented serialization of environmental telemetry (temperature, humidity, TVOC, eCO2, and pressure) to the WebSocket status packet payload.

### [0.1.47] — 2026-07-04
- Added I2C polling support for LED Tower module type (0x08).
- Implemented serialization of LED Tower brightness state to the WebSocket status packet payload.

### [0.1.46] — 2026-07-02
- Resolved clangd warnings in data_broker.c by fixing float-precision casts and removing unused crt header.

### [0.1.45] — 2026-07-01
- Reduced I2C polling interval to 1.0 second and optimized discovery routine to run only once every 5 seconds, resulting in immediate metadata and song updates.
- Refined remaining track time interpolation to compute it based on total duration, eliminating fluctuations.

### [0.1.44] — 2026-07-01
- Redesigned progression interpolation to filter out stale/lagging AVRCP play position packets and prevent the progress bar from jumping back and forth.

### [0.1.43] — 2026-06-30
- Reverted I2C polling interval to 2.0s and WebSocket broker interval to 1.0s to prevent I2C bus timeouts and collisions.
- Implemented client-side progress interpolation in the web player for smooth 1-second track updates.
- Embedded changelog in the web client as a fallback to resolve fetch errors when loaded via HTTP.

### [0.1.42] — 2026-06-29
- Reduced I2C polling and WebSocket data broker intervals to 500ms for near real-time dashboard updates.

### [0.1.41] — 2026-06-29
- Reduced I2C discovery and status polling interval to 1.0 second for smoother dashboard updates.

### [0.1.40] — 2026-06-29
- Fixed FOTA status polling task lockup: added checking for `OTA_STATE_IDLE` to break from the `module_ota_task` loop. This prevents the Master from getting stuck polling the Slave's OTA status, which paused regular I2C manager polling and blocked all manual player controls and metadata updates.
- Improved web player UI layout to display Title, Artist, and Album separately.

### [0.1.39] — 2026-06-24
- Implemented polling and WebSocket forwarding for Bluetooth A2DP/AVRCP track metadata (title, artist, album) from the speaker slave module.

### [0.1.38] — 2026-06-22
- Rearchitected module firmware update process: replaced I2C chunk streaming with direct Wi-Fi FOTA updates, transmitting Wi-Fi credentials and firmware URLs to target slaves.
- Implemented status polling loop and WebSocket broadcast to update the frontend UI.

### [0.1.37] — 2026-06-15
- Implemented robust transmit-then-poll streaming protocol for module OTA updates over I2C, resolving communication NACK errors during slave flash write operations.

### [0.1.36] — 2026-06-15
- Paused I2C manager early in the module OTA task to avoid concurrent polling during the HTTP connection/handshake phase.

### [0.1.35] — 2026-06-15
- Implemented dynamic I2C OTA phase-shift recovery: detects if the Slave sends a stale/misaligned response chunk index (due to a previous timeout) and performs a clean read transaction without transmitting, clearing the stale queue and restoring proper frame alignment.
- Allowed `i2c_manager_send_frame` to perform read-only transactions (when `tx` is NULL) on both standard and no-offset addresses.

### [0.1.34] — 2026-06-15
- Refined I2C read length behavior: allowed reading extra bytes (`rx_len + 48`) during non-data commands (like `SUB_OTA_BEGIN` and `SUB_OTA_END`) to clear any stale frames from the Slave's TX FIFO.

### [0.1.33] — 2026-06-15
- Added compatibility delay (220ms wait) for older Slave modules (versions below 20/0.1.10) to accommodate slower flash writes and LED refreshes during I2C streaming updates.

### [0.1.32] — 2026-06-15
- Implemented I2C polling optimizations and WebSocket message handling for Bluetooth Speaker controls.

### [0.1.31] — 2026-06-15
- Minor internal updates.

### [0.1.20] — 2026-06-12
- Increased default I2C command response wait time to 50ms for improved communication stability.

### [0.1.19] — 2026-06-12
- Fixed I2C OTA delay logic for legacy slave firmware.

### [0.1.18] — 2026-06-12
- Implemented dynamic delay compatibility for slave modules running v0.1.6.

### [0.1.17] — 2026-06-12
- Optimized Base OTA_BEGIN wait time to 5000ms.

### [0.1.16] — 2026-06-12
- Fixed I2C OTA Chunk 0 timeout: removed slave 200ms delay and increased master wait to 30ms.

### [0.1.15] — 2026-06-12
- Increased HTTP client buffer size to 4096 bytes to support long redirect URLs from GitHub releases to AWS S3.
- Implemented HTTP client redirection handling (up to 5 redirects).

### [0.1.14] — 2026-06-12
- Preparatory updates for HTTP client redirection.

### [0.1.13] — 2026-06-12
- Initial release containing I2C polling pause during Slave OTA updates and orange blinking LED support.

### [0.1.12] — 2026-06-10
- Implemented real I2C OTA firmware streaming protocol over I2C (BEGIN/DATA/END sub-commands).
- Configured dynamic I2C response wait times depending on the specific sub-command.
- Increased `module_ota_task` stack size to support full HTTPS download and streaming.

### [0.1.11] — 2026-06-10
- Bumped project version to coordinate with module update.

### [0.1.10] — 2026-06-15
- Implemented CMD_GET_INFO (0x02) request in polling loop and post-OTA sequence to refresh module type and versions.

### [0.1.9] — 2026-06-15
- Increased CMD_FIRMWARE_UPDATE I2C response delay to 50ms to ensure slave NVS write completion.

### [0.1.8] — 2026-06-10
- Added support for asynchronous I2C module OTA progress and changelog rendering.
- Resolved I2C response length validation issues for module firmware update command.
- Display specific module type and version in web application.
- Changed default base IP to 192.168.1.178.

### [0.1.7] — 2026-06-10
- Added `CMD_GET_VBUS_VOLTAGE` I2C command to query VBUS voltage readings from connected modules.
- Implemented module-assisted USB-PD power negotiation when the Base's internal ADC is unavailable.

### [0.1.6] — 2026-06-10
- Handled invalid VBUS ADC pin on Base gracefully to prevent initialization errors.
- Set default Base VBUS voltage reporting to 5V (5000 mV) when the internal ADC is not available.

### [0.1.5] — 2026-06-10
- Fixed HTTP client buffer overflow error during FOTA updates by increasing buffer size.
- Fixed front-end JavaScript console exception when WebSocket connection is closed.

### [0.1.4] — 2026-06-10
- Incremented version to 0.1.4 for FOTA update flow verification.

### [0.1.3] — 2026-06-10
- Added real-time FOTA update progress tracking and detailed error reporting to the app dashboard.
- Configured asynchronous background task for OTA updates to avoid blocking WebSockets.
- Added default SSL CA certificate bundle (`esp_crt_bundle.h`) to verify HTTPS connections.

### [0.1.2] — 2026-06-10
- Added VBUS voltage monitoring on serial output every 10 seconds.

### [0.1.1] — 2026-06-03
- Bumped software version of Modulo Base (`base_app`) to `0.1.1`.
- Updated version display and formatting in mobile app dashboard and update manager.

### [0.1.0] — 2026-06-03
- First functional firmware release for the Modulo Base.
- Wi-Fi provisioning via BLE (NimBLE).
- OTA firmware update support with dual OTA partitions and rollback.
- WebSocket-based communication with the mobile app.
- Module hot-swap detection and lifecycle management.
- Power distribution and communication bus management.

---

## Modulo Bluetooth Speaker

### [0.1.29] — 2026-07-04
- Spared and bumped version to 0.1.29 to keep aligned with LED Tower release.

### [0.1.28] — 2026-07-03
- Migrated default pinout mappings for ESP32 target (SDA=27, SCL=14, ADC=36, LED=19, I2S standard pins) to align with the new hardware revision schematic.

### [0.1.27] — 2026-07-01
- Filtered out 0xFFFFFFFF play status query payloads from the phone to prevent tracking errors.

### [0.1.26] — 2026-06-30
- Reverted periodic Bluetooth AVRCP status query interval to 1.0 second to ensure system stability and avoid bus conflicts.

### [0.1.25] — 2026-06-29
- Reduced AVRCP play status query interval to 500ms for near real-time track progress updates.

### [0.1.24] — 2026-06-29
- Reduced periodic AVRCP play status query interval to 1.0 second for smoother real-time track progress.

### [0.1.23] — 2026-06-29
- Implemented classic Bluetooth AVRCP play status polling (position and duration queries) and exposed progress and remaining track time values over I2C status packet.

### [0.1.22] — 2026-06-29
- Freed Bluetooth and I2S resources during OTA to prevent Out-Of-Memory (OOM) heap segment allocation freeze at 4% progress.

### [0.1.21] — 2026-06-29
- Migrated legacy I2C Slave driver to the new ESP-IDF v5.x driver (`driver/i2c_slave.h`).

### [0.1.20] — 2026-06-29
- Cleaned up Bluetooth compiler warnings.

### [0.1.15] — 2026-06-24
- Reset Slave OTA state to IDLE upon receiving SSID.

### [0.1.14] — 2026-06-24
- Implemented full Bluetooth Classic A2DP Sink and AVRCP Controller stack.
- Configured software-based volume control using Q8 fixed-point quadratic curve.
- Enabled track metadata transmission via new `CMD_SPK_GET_METADATA` command.
- Integrated aggressive compile-size and IRAM optimizations to prevent memory segment overflow.

### [0.1.13] — 2026-06-22
- Test release for FOTA update verification with 3-byte SemVer protocol.

### [0.1.12] — 2026-06-22
- Implemented direct Wi-Fi FOTA update capability: connects to Wi-Fi STA and executes HTTPS OTA in a background task, keeping the I2C bus responsive.

### [0.1.11] — 2026-06-15
- Implemented automatic I2C driver recovery to clear hardware bus lockups/hangs: distinguished driver/communication errors (negative return values) from standard idle timeouts. When an error is detected, the Slave automatically de-initializes and re-initializes its I2C driver to reset the hardware peripheral and release the SDA/SCL lines.

### [0.1.10] — 2026-06-15
- Implemented Bluetooth volume control (0-100), song titles, progress tracking, and listening states.
- Optimized I2C OTA updates by throttling LED status strip refresh rate (once every 500ms) to eliminate high latency and avoid blocking I2C transactions.

### [0.1.9] — 2026-06-15
- Skeletons and commands for bluetooth controls (play, pause, next, prev, volume up/down).

### [0.1.8] — 2026-06-10
- Skeletons for bluetooth controls.

### [0.1.7] — 2026-06-10
- Skeletons and build configurations.

### [0.1.5] — 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.
- Switched partition table layout to two-OTA partitions.

### [0.1.4] — 2026-06-10
- Removed `s_module_type` loading from NVS to prevent stale/incorrect module type reports.

### [0.1.3] — 2026-06-10
- Implemented CMD_GET_INFO (0x02) command to return unique chip ID, module type, and hardware/software versions.

### [0.1.2] — 2026-06-10
- Deferred NVS flash writes in CMD_FIRMWARE_UPDATE response to prevent blocking the I2C transaction.

### [0.1.1] — 2026-06-03
- Bumped software version of Modulo Slave (`slave_app`) to `0.1.1`.
- Updated version display and formatting.

### [0.0.1] — 2026-06-03
- Initial setup.

---

## Modulo Environmental Monitor

### [0.1.42] — 2026-07-08
- **Added sensor power stabilization delay**: Added a 150ms delay at the very beginning of `sensor_mgr_init` before performing any I2C communication. This allows the 3.3V power rail and the ENS160 internal boot logic to stabilize on cold power-on, preventing the sensor from getting locked in its bootloader state by premature I2C transactions.

### [0.1.41] — 2026-07-08
- **Fixed ENS160 mode transition lock**: Added a NOP (0x00) command write to the `COMMAND` register (0x12) after version retrieval and before setting STANDARD mode. The ENS160 internal command handler requires the register to return to NOP to allow the OPMODE transition from IDLE to STANDARD.

### [0.1.40] — 2026-07-08
- **Fixed ENS160 boot ready detection**: Changed bootloader ready wait logic to issue the official `GET_APPVER` command (0x0E) and verify the version returns a non-zero value, ensuring we wait until the sensor has fully loaded its internal firmware before starting standard mode.

### [0.1.39] — 2026-07-08
- **Added hardware diagnostics and raw data logging**: Added an EPD RST-to-BUSY pin loopback test at startup to verify physical connections and power. Added pre-init register dump for the ENS160 gas sensor, along with raw reading logs for both AHT21 (temp/humidity) and ENS160 (TVOC/eCO2 bytes) to isolate and diagnose sensor and display activity.

### [0.1.38] — 2026-07-08
- **Fixed OTA abort/reset crash**: Added a 300ms delay after deleting `env_sensor_task` at the start of FOTA. This gives the FreeRTOS idle task time to actually deallocate the 8KB task stack before starting the heavy Wi-Fi driver, avoiding Out-Of-Memory (OOM) allocations.

### [0.1.37] — 2026-07-08
- **Fixed ENS160 boot ready lock**: Implemented the official ScioSense startup handshake loop (NOP 0x00 + CLRGPR 0xCC, reading GPR_READ_4..6 (0x4C) until they are all 0) to ensure the internal MCU bootloader is ready before transitioning the chip into continuous measurement mode. Also guaranteed the mandatory transition from IDLE to STANDARD mode during automatic register checks/restores.
- **Fixed e-Paper blank screen**: Added the official Waveshare V2 Look-Up Table (LUT) loading sequence for both full (WS_20_30) and partial (_WF_PARTIAL_2IN9) refresh modes, enabling the SSD1680 controller to generate proper driving voltages.

### [0.1.36] — 2026-07-08
- **Fixed ENS160 measurement engine not starting**: Added mandatory COMMAND register writes (NOP 0x00 + CLRGPR 0xCC to register 0x12) during sensor initialization. Without these commands, the sensor accepts OPMODE=STANDARD but never activates its internal heater/measurement engine (STATAS bit stays 0), resulting in perpetual TVOC=0, eCO2=0, AQI=0 readings.
- **Fixed e-Paper display not refreshing**: Corrected Display Update Control 2 values to match official Waveshare V2 driver (0xC7 for full refresh, 0x0F for partial). Previous values (0xF7/0xFF) caused the SSD1680 controller to use invalid waveform sequences. Also added 10ms delay after SW reset per datasheet requirements, and RAM address counter initialization (0x4E/0x4F) at end of init sequence.

### [0.1.35] — 2026-07-08
- Added verbose diagnostics for ENS160 (logs PART_ID, OPMODE, and status registers on every measurement cycle).
- Added return code verification and error logs for e-Paper initialization (`epd_init`) in the main sensor task.

### [0.1.34] — 2026-07-08
- Fixed E-Paper false-positive "restored" logs: corrected EPD error telemetry status so the busy timeout flag persists until a refresh command completes successfully.
- Added detailed telemetry debugging to trace startup/refresh GPIO levels of the busy pin.
- Optimized hardware reset timing for WaveShare SSD1680 displays (toggled RST pin with 200ms intervals).

### [0.1.33] — 2026-07-07
- Fixed FOTA crash/abort on standard ESP32: automatically pauses/deletes the heavy environmental sensor and e-Paper drawing tasks before starting Wi-Fi STA and HTTPS OTA, reclaiming 8KB of task stack and preventing Out-Of-Memory (OOM) failures.
- Displays a dedicated "AGGIORNAMENTO FIRMWARE..." message on the e-Paper display during the update process.

### [0.1.32] — 2026-07-07
- Added I2C and EPD error flags (AHT21 connection, ENS160 connection, ENS160 data validity, EPD busy timeout) to the protocol payload.

### [0.1.31] — 2026-07-07
- Added a 50ms boot delay after ENS160 I2C reset command to avoid register lockups.
- Improved e-Paper (SSD1680) driver: implemented proper reset sequence (HIGH-LOW-HIGH), pre-initialized pin levels to prevent startup glitches, and reduced SPI clock to 2MHz for high stability.
- Alternates between fast partial refreshes and periodic full refreshes (once per minute) to avoid screen damage and flickering.

### [0.1.30] — 2026-07-07
- Implemented real-time AQI and eCO2 reading from ENS160 registers and removed simulated sensor fallback.
- Added e-Paper graphic drawing sections for real-time AQI levels and the list of detected gases (VOCs, Toluene, Hydrogen, Ethanol, NO2, Ozone).
- Integrated dual I2C address detection (auto-scanning addresses 0x53 and 0x52) for ENS160.
- Disabled SPI DMA to support safe stack-allocated transactions and resolved e-Paper boot display issue.
- Updated base station serialization and web dashboard interface to display AQI, eCO2, and monitored gas details.
- Removed deprecated atmospheric pressure fields.

### [0.1.29] — 2026-07-07
- Implemented first software release of environmental monitor telemetry and display interface.
- Configured local I2C Master bus on GPIO 21 (SDA) and GPIO 22 (SCL) to read data from ENS160 (TVOC/eCO2) and AHT21 (Temp/Hum) sensors, with automated simulation fallback.
- Configured VSPI bus on GPIO 23 (DIN), GPIO 18 (CLK), GPIO 5 (CS), GPIO 25 (DC), GPIO 26 (RST), and GPIO 32 (BUSY) to drive Waveshare 2.9" portrait e-Paper display.
- Implemented graphics drawing library (with customized thermometer, droplet, wind, and CO2 cloud icons) and progressive TVOC bar graph.
- Exposed telemetry packet structured data (env_sensor_data_t) to Base station via CMD_GET_STATUS (0x03) over I2C Slave interface.

### [0.1.6] — 2026-06-15
- Skeletons and build configurations.

### [0.1.5] — 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.

### [0.1.4] — 2026-06-10
- Removed `s_module_type` loading from NVS.

### [0.1.3] — 2026-06-10
- Implemented CMD_GET_INFO (0x02) command.

### [0.1.2] — 2026-06-10
- Deferred NVS flash writes.

---

## Modulo Smart Screen 128

### [0.1.6] — 2026-06-15
- Skeletons and build configurations.

### [0.1.5] — 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.

### [0.1.4] — 2026-06-10
- Removed `s_module_type` loading from NVS.

### [0.1.3] — 2026-06-10
- Implemented CMD_GET_INFO (0x02) command.

### [0.1.2] — 2026-06-10
- Deferred NVS flash writes.

---

## Modulo USB-C Charger

### [0.1.6] — 2026-06-15
- Skeletons and build configurations.

### [0.1.5] — 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.

### [0.1.4] — 2026-06-10
- Removed `s_module_type` loading from NVS.

### [0.1.3] — 2026-06-10
- Implemented CMD_GET_INFO (0x02) command.

### [0.1.2] — 2026-06-10
- Deferred NVS flash writes.

---

## Modulo Wireless Charger

### [0.1.6] — 2026-06-15
- Skeletons and build configurations.

### [0.1.5] — 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.

### [0.1.4] — 2026-06-10
- Removed `s_module_type` loading from NVS.

### [0.1.3] — 2026-06-10
- Implemented CMD_GET_INFO (0x02) command.

### [0.1.2] — 2026-06-10
- Deferred NVS flash writes.

---

## Modulo LED Tower

### [0.1.29] — 2026-07-04
- Implemented LEDC (PWM) controller on GPIO 19 (EXT_LEDOUT) to allow dimmer control (0-100% duty cycle).
- Added I2C commands handler for CMD_SET_STATE (to set dimmer brightness) and CMD_GET_STATUS (to retrieve current brightness).

### [0.1.6] — 2026-06-15
- Skeletons and build configurations.

### [0.1.5] — 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.

### [0.1.4] — 2026-06-10
- Removed `s_module_type` loading from NVS.

### [0.1.3] — 2026-06-10
- Implemented CMD_GET_INFO (0x02) command.

### [0.1.2] — 2026-06-10
- Deferred NVS flash writes.
