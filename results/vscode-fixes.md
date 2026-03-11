# VS Code Fixes

## Fix 1: BusyBox Bash Adaptation (LOW)

- **Issue:** [#170868](https://github.com/microsoft/vscode/issues/170868)
- **PR:** [#300900](https://github.com/microsoft/vscode/pull/300900)

### Problem

VS Code's shell integration script failed on BusyBox environments where bash behaves differently. The script assumed a full bash implementation.

### Change

Added an early-return guard in `shellIntegration-bash.sh` that checks `BASH_VERSION` before executing bash-specific integration logic. If the variable is unset or indicates an incompatible shell, the script exits gracefully.

### Files Changed

| File | Change |
|---|---|
| `shellIntegration-bash.sh` | Added `BASH_VERSION` guard with early return |

### Validation

- `npm run compile` -- passed

---

## Fix 2: x64 Auto-Update on ARM64 Windows (MED)

- **Issue:** [#141031](https://github.com/microsoft/vscode/issues/141031)
- **PR:** [#300902](https://github.com/microsoft/vscode/pull/300902)

### Problem

When running the x64 build of VS Code on ARM64 Windows (via emulation), the auto-updater would download ARM64 builds, breaking the installation. Users needed to stay on x64 but the update service did not detect the architecture mismatch.

### Changes

| File | Change |
|---|---|
| Update service types | Added `DisablementReason.RunningX64OnArm64` enum value |
| `nativeHostMainService` | Updated `isRunningUnderARM64Translation()` to detect x64-on-ARM64 Windows |
| `Win32UpdateService` | Disabled auto-update when running x64 on ARM64 |
| Workbench banner | Added informational banner via `BannerService` explaining the situation |
| Tooltip | Updated tooltip text to indicate why updates are disabled |

### Validation

- `npm run compile-check-ts-native` -- passed
- Build typecheck -- passed
