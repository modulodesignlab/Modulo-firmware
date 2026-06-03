# Firmware Versioning

This document describes the versioning strategy used across all Modulo firmware, including the Base and all Smart Modules.

---

## Semantic Versioning

Modulo firmware follows [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html). Every firmware version is expressed as:

```
MAJOR.MINOR.PATCH
```

Each component of the version number conveys specific meaning about the nature and scope of changes included in the release.

---

## Version Components

### Major (`X.0.0`)

A **major** version increment indicates **breaking changes** that are incompatible with previous versions.

Examples:

- Redesign of the communication protocol between Base and modules.
- Removal of a previously supported feature or API.
- Changes to the flash memory layout that prevent rollback to earlier versions.

> ⚠️ Major updates may require all modules to be updated together with the Base. Users will be notified through the Modulo mobile app before a major update is applied.

### Minor (`0.X.0`)

A **minor** version increment indicates **new functionality** that is backward-compatible with the previous minor version.

Examples:

- Addition of a new sensor reading mode for the Environmental Monitor.
- Support for a new display layout on the Smart Screen.
- New LED animation patterns for the LED Tower.
- Performance improvements to the wireless charging algorithm.

Minor updates are safe to install independently and do not require coordinated updates across devices.

### Patch (`0.0.X`)

A **patch** version increment indicates **bug fixes** and minor corrections that are backward-compatible.

Examples:

- Fix for a Bluetooth audio dropout under specific conditions.
- Correction of an incorrect temperature reading offset.
- Resolution of a rare crash during module hot-swap.
- Stability improvements to the Wi-Fi connection manager.

Patch updates are low-risk and are applied automatically.

---

## Compatibility Strategy

The Modulo ecosystem consists of independently versioned firmware components. Ensuring compatibility between the Base and each module is critical to system stability.

### Independent Versioning

Each device (Base and modules) maintains its own version number. Versions are incremented independently as each device receives its own updates. There is no requirement for version numbers to be synchronized across devices.

### The `minimum_base_version` Field

Module firmware may depend on features or protocols provided by the Base. To express this dependency, each module's manifest entry includes a `minimum_base_version` field.

```json
{
  "bluetooth_speaker": {
    "version": "1.2.0",
    "minimum_base_version": "1.1.0",
    "url": "...",
    "sha256": "..."
  }
}
```

**Rules:**

| Condition | Behavior |
|---|---|
| Base version ≥ `minimum_base_version` | Module update proceeds normally. |
| Base version < `minimum_base_version` | Module update is **deferred** until the Base is updated to at least the required version. |
| Base update is available | The Base is always updated **first**, before any module updates. |

The Base firmware does **not** have a `minimum_base_version` field since it has no dependency on itself.

### Update Ordering

When both Base and module updates are available in the same cycle, the system follows this order:

1. **Base firmware** is updated and verified first.
2. **Module firmware** updates are applied after the Base update is confirmed.
3. If a module update's `minimum_base_version` still isn't met after the Base update, that module update is skipped until a future Base release satisfies the requirement.

---

## Best Practices

### For Firmware Developers

1. **Increment versions deliberately.** Choose the correct version component (major, minor, patch) based on the nature of the change. When in doubt, prefer a minor increment over a patch.

2. **Set `minimum_base_version` conservatively.** Only raise the minimum Base version requirement when the module genuinely depends on new Base features or protocol changes. Unnecessary increases delay module updates for users who haven't updated their Base.

3. **Never reuse a version number.** Once a version is published, it must not be reassigned to a different build. If a release is defective, publish a new patch version.

4. **Document breaking changes.** Major version increments must be accompanied by clear changelog entries and release notes explaining what changed and why.

5. **Test compatibility.** Before publishing a module firmware update, verify that it functions correctly with the declared `minimum_base_version` and with the latest Base version.

### For Release Management

1. **Update the manifest atomically.** When publishing a new firmware version, update `manifest.json`, attach the binary to a GitHub Release, and update `changelog.md` in a single commit.

2. **Compute checksums at build time.** The SHA-256 hash must be computed from the exact binary that is uploaded to the GitHub Release. Never modify the binary after computing the hash.

3. **Tag releases.** Use Git tags in the format `<device>-v<version>` (e.g., `base-v1.0.0`, `bluetooth_speaker-v1.2.0`) to mark the commit associated with each firmware release.

4. **Maintain the changelog.** Every version published to the manifest must have a corresponding entry in `changelog.md` with a clear description of changes.

---

## Version Lifecycle

```
Development ──▶ Internal Testing ──▶ Release ──▶ Active ──▶ Deprecated
                                        │
                                        ├── manifest.json updated
                                        ├── GitHub Release published
                                        └── changelog.md updated
```

| Stage | Description |
|---|---|
| **Development** | Firmware is under active development in the private source repository. |
| **Internal Testing** | Firmware is tested on internal hardware before public release. |
| **Release** | Firmware is published to this repository and becomes available for OTA updates. |
| **Active** | Firmware is the current recommended version for the device. |
| **Deprecated** | Firmware has been superseded by a newer version. Devices running deprecated firmware will be prompted to update. |

---

_Last updated: 2026-06-03_
