# DefinitelyTyped Fixes

## Fix 1: scheduler -- Missing 0.27 Exports (LOW)

- **Package:** `scheduler`
- **PR:** [#74674](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/74674)

### Problem

The `@types/scheduler` package was missing exports introduced in React scheduler 0.27. Subpath exports for `unstable_mock` and `unstable_post_task` were not declared.

### Changes

| File | Change |
|---|---|
| `index.d.ts` | Updated exports to include 0.27 API surface |
| `package.json` | Bumped version to `0.27.9999` |
| `unstable_mock.d.ts` | Added subpath type declarations |
| `unstable_post_task.d.ts` | Added subpath type declarations |
| Test files | Updated tests to cover new exports |

**Total:** 6 files changed

### Validation

- `dtslint` -- passed

---

## Fix 2: node-notifier -- v10 Type Alignment (MED)

- **Package:** `node-notifier`
- **PR:** [#74675](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/74675)
- **Commit:** `012eda6`

### Problem

The `@types/node-notifier` package was out of sync with node-notifier v10. Multiple notifier classes were missing constructor aliases, `EventEmitter` inheritance, and several options fields.

### Changes

| File | Change |
|---|---|
| Notifier class definitions (5) | Added `Notification` constructor alias, `EventEmitter` inheritance |
| Options interfaces | Added missing fields: `app-name`, `appName`, `actions`, passthrough flags |
| Test files | Updated tests to cover new constructors and options |

**Total:** 7 files changed

### Validation

- `dtslint` -- passed

### Commit

Includes DCO sign-off.
