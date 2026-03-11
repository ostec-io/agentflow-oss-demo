# Sprint 2: Issue Scouting

## Scoring Rubric

Each candidate issue was scored on three dimensions (1-3 scale):

| Dimension | 1 (Low) | 2 (Medium) | 3 (High) |
|---|---|---|---|
| Files touched | 1-2 files | 3-5 files | 6+ files |
| Architecture knowledge | Isolated change | Some subsystem awareness | Cross-cutting concerns |
| Test complexity | Existing tests cover it | New test cases needed | Integration/E2E tests |

Issues were classified as **LOW** (total 3-4) or **MED** (total 5-7) difficulty.

## Selected Issues

### VS Code

| Difficulty | Issue | Title | Score Rationale |
|---|---|---|---|
| LOW | [#170868](https://github.com/microsoft/vscode/issues/170868) | BusyBox bash adaptation | 1 file, clear fix path from maintainer comment |
| MED | [#141031](https://github.com/microsoft/vscode/issues/141031) | x64 auto-update on ARM64 Windows | 6+ files, update service + banner changes |

### Kubernetes

| Difficulty | Issue | Title | Score Rationale |
|---|---|---|---|
| LOW | [#137520](https://github.com/kubernetes/kubernetes/issues/137520) | Missing sample-device-plugin:1.7 in test manifest | 2 files, straightforward enum addition |
| MED | [#93474](https://github.com/kubernetes/kubernetes/issues/93474) | kubectl --request-timeout breaks in-cluster config | 4 files, fragile config loading path |

### DefinitelyTyped

| Difficulty | Issue | Title | Score Rationale |
|---|---|---|---|
| LOW | scheduler | Missing 0.27 exports | 6 files, straightforward type sync |
| MED | node-notifier | v10 type alignment | 7 files, multi-notifier class update |

## Human Gate

After scouting completed, the 6 issue selections were presented for human approval. This was the **only manual intervention** in the entire pipeline. The user reviewed the issue list and approved all 6 candidates to proceed to Sprint 3.
