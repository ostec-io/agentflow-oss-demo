# Kubernetes Fixes

## Fix 1: Missing sample-device-plugin:1.7 in Test Manifest (LOW)

- **Issue:** [#137520](https://github.com/kubernetes/kubernetes/issues/137520)
- **PR:** [#137644](https://github.com/kubernetes/kubernetes/pull/137644)
- **Commit:** `8c41cef0de9`

### Problem

The e2e test image manifest was missing the `sample-device-plugin:1.7` image entry, causing test validation scripts to fail when referencing this image.

### Changes

| File | Change |
|---|---|
| `manifest.go` | Added `SampleDevicePlugin` to the `ImageID` enum |
| Image config | Added config entry for `sample-device-plugin:1.7` |
| `.permitted-images` | Added the image to the permitted list |

### Validation

- `go test` -- passed
- `hack/verify-e2e-images.sh` -- passed

### Commit

Includes `Signed-off-by` line per CNCF DCO requirements.

---

## Fix 2: kubectl --request-timeout Breaks In-Cluster Config (MED)

- **Issue:** [#93474](https://github.com/kubernetes/kubernetes/issues/93474)
- **PR:** [#137645](https://github.com/kubernetes/kubernetes/pull/137645)
- **Commit:** `d5cbda98570`

### Problem

When `kubectl` is invoked with `--request-timeout` from within a pod using in-cluster config (service account token), the timeout flag was not applied. The root cause was that `DefaultClientConfig` was built without the same command-line overrides, so in-cluster config paths did not inherit CLI flags.

### Changes

| File | Change |
|---|---|
| `config_flags.go` | Updated `DefaultClientConfig` construction to include the same overrides as out-of-cluster config |
| `client_config.go` | Added `applyConfigOverrides` function that replays CLI overrides for in-cluster config |
| Test files (3) | Added unit tests covering timeout propagation in in-cluster scenarios |

### Validation

- `go test` on affected packages -- passed

### Commit

Includes `Signed-off-by` line per CNCF DCO requirements.
