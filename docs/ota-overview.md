# OTA Update Overview

This document describes the over-the-air (OTA) firmware update architecture used by the Modulo ecosystem.

---

## Architecture

The Modulo OTA system follows a **pull-based** architecture. The Modulo Base periodically checks for firmware updates by fetching a centralized manifest file hosted in this repository. When a new version is detected for the Base or any connected module, the Base orchestrates the download, validation, and installation of the firmware.

```
┌─────────────────────────────────────────────────────────────┐
│                        Modulo Base                          │
│                                                             │
│  ┌─────────────┐    ┌──────────────┐    ┌───────────────┐  │
│  │ Update       │───▶│ Manifest     │───▶│ Firmware      │  │
│  │ Scheduler    │    │ Parser       │    │ Downloader    │  │
│  └─────────────┘    └──────────────┘    └───────┬───────┘  │
│                                                   │         │
│                                          ┌───────▼───────┐  │
│                                          │ Integrity     │  │
│                                          │ Validator     │  │
│                                          └───────┬───────┘  │
│                                                   │         │
│                      ┌────────────────────────────▼──────┐  │
│                      │ Flash Manager (Base + Modules)    │  │
│                      └───────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
        ┌──────────┐   ┌──────────┐   ┌──────────┐
        │ Module A │   │ Module B │   │ Module C │
        └──────────┘   └──────────┘   └──────────┘
```

### Key Components

| Component | Role |
|---|---|
| **Update Scheduler** | Triggers periodic update checks over Wi-Fi. |
| **Manifest Parser** | Fetches and parses `manifest.json` from this repository. |
| **Firmware Downloader** | Downloads firmware binaries from the URLs in the manifest. |
| **Integrity Validator** | Verifies downloaded binaries against the SHA-256 checksum. |
| **Flash Manager** | Writes validated firmware to the Base or the target module via the communication bus. |

---

## How Devices Check for Updates

1. The Modulo Base connects to the internet via Wi-Fi.
2. On a configurable interval (default: every 24 hours), the Update Scheduler initiates an update check.
3. The Base fetches the latest `manifest.json` from this repository's raw URL:
   ```
   https://raw.githubusercontent.com/modulodesignlab/modulo-firmware/main/manifest.json
   ```
4. The Manifest Parser reads the file and compares the `version` field for each firmware entry against the currently installed version on the Base and all connected modules.
5. If a newer version is available for any device, the update process is triggered.

Users may also trigger a manual update check through the Modulo mobile app.

---

## Manifest Usage

The `manifest.json` file is the central registry of all available firmware versions. It uses the following schema:

```json
{
  "schema_version": 1,
  "firmwares": {
    "<device_key>": {
      "version": "<semver>",
      "minimum_base_version": "<semver>",
      "url": "<download_url>",
      "sha256": "<hex_digest>"
    }
  }
}
```

### Fields

| Field | Type | Description |
|---|---|---|
| `schema_version` | `integer` | Manifest schema version. Currently `1`. |
| `firmwares` | `object` | Map of device keys to their firmware metadata. |
| `version` | `string` | The latest available firmware version (Semantic Versioning). |
| `minimum_base_version` | `string` | The minimum Base firmware version required for compatibility. **Omitted for the Base itself.** |
| `url` | `string` | Direct download URL for the firmware binary (typically a GitHub Release asset). |
| `sha256` | `string` | SHA-256 hex digest of the firmware binary for integrity validation. |

### Device Keys

| Key | Device |
|---|---|
| `base` | Modulo Base |
| `bluetooth_speaker` | Bluetooth Speaker Module |
| `environmental_monitor` | Environmental Monitor Module |
| `smart_screen_128` | 1.28" Smart Screen Module |
| `usb_c_charger` | USB-C Device Charger Module |
| `wireless_charger` | Wireless Charger Module |
| `led_tower` | LED Tower Module |

---

## Firmware Download Process

When an update is available for a device, the Base executes the following steps:

1. **Compatibility Check** — For module firmware, verify that the Base's current firmware version meets or exceeds the `minimum_base_version` specified in the manifest. If the Base itself needs an update, prioritize the Base update first.

2. **Download** — Fetch the firmware binary from the `url` specified in the manifest. Downloads use HTTPS to ensure transport-layer security.

3. **Integrity Validation** — Compute the SHA-256 hash of the downloaded binary and compare it against the `sha256` value in the manifest. If the hashes do not match, the binary is discarded and the update is retried on the next cycle.

4. **Installation** — Write the validated binary to the target device:
   - For the **Base**, the firmware is written to internal flash and the device reboots.
   - For **Modules**, the firmware is transmitted over the communication bus to the target module's microcontroller.

5. **Verification** — After installation, the device reports its new firmware version. The Base confirms the update was successful.

```
Check Manifest ──▶ Version Comparison ──▶ Compatibility Check
                                                │
                                         ┌──────▼──────┐
                                         │   Download   │
                                         └──────┬──────┘
                                                │
                                         ┌──────▼──────┐
                                         │  SHA-256     │
                                         │  Validation  │
                                         └──────┬──────┘
                                                │
                                    ┌───────────▼───────────┐
                                    │  Flash & Verify       │
                                    └───────────────────────┘
```

---

## SHA-256 Validation

Every firmware binary distributed through this repository includes a SHA-256 checksum in the manifest. This checksum serves as an integrity guarantee.

### Process

1. Before a firmware release is published, the SHA-256 hash of the compiled binary is computed.
2. The hash is recorded in the `sha256` field of the corresponding manifest entry.
3. After download, the Modulo Base independently computes the SHA-256 hash of the received file.
4. The computed hash is compared against the manifest value.
5. **If the hashes match**, the firmware is considered intact and installation proceeds.
6. **If the hashes do not match**, the firmware is rejected. The Base logs the error and retries on the next update cycle.

This mechanism protects against:

- Corrupted downloads due to network issues.
- Tampered firmware binaries (basic integrity assurance).
- Incomplete or truncated transfers.

---

## Future: Signed Firmware

In a future release, Modulo will implement **cryptographic firmware signing** to provide stronger security guarantees beyond integrity validation.

### Planned Features

- **Asymmetric Signatures** — Each firmware binary will be signed with a private key held by Modulo Design Lab. Devices will verify the signature using a public key embedded in the bootloader.
- **Certificate Chain** — A certificate chain will be used to establish trust between the signing authority and the device.
- **Rollback Protection** — Devices will refuse to install firmware with a version number lower than the currently installed version, preventing downgrade attacks.
- **Manifest Signing** — The `manifest.json` file itself will be signed, ensuring that the manifest has not been tampered with in transit.

The manifest schema will be updated to include signature fields when this feature is implemented. The `schema_version` field will be incremented accordingly.

---

_Last updated: 2026-06-03_
