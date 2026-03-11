# Sprint 4: PR Submission

## Structure

3 sprints (one per project), each containing 2 parallel tasks for the LOW and MED fixes. All 6 tasks completed in approximately 4 minutes.

## PR Conventions by Repository

### VS Code

- **Title format:** `fix: <description> (fixes #<number>)`
- **Reviewer assignment:** None -- team triage handles routing
- **CLA:** Microsoft CLA (signed at org level)

### Kubernetes

- **Body format:** What / Why / How sections with `Fixes #<number>`
- **Post-create commands:** `/sig <name>` and `/kind <type>` prow bot comments
- **Commits:** `Signed-off-by` line required (DCO)

### DefinitelyTyped

- **Title format:** `[package-name] description`
- **Review:** dt-mergebot auto-reviews
- **Commits:** DCO sign-off required

## Submitted PRs

| Repository | Issue | PR | Difficulty |
|---|---|---|---|
| microsoft/vscode | #170868 | [#300900](https://github.com/microsoft/vscode/pull/300900) | LOW |
| microsoft/vscode | #141031 | [#300902](https://github.com/microsoft/vscode/pull/300902) | MED |
| kubernetes/kubernetes | #137520 | [#137644](https://github.com/kubernetes/kubernetes/pull/137644) | LOW |
| kubernetes/kubernetes | #93474 | [#137645](https://github.com/kubernetes/kubernetes/pull/137645) | MED |
| DefinitelyTyped/DefinitelyTyped | scheduler | [#74674](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/74674) | LOW |
| DefinitelyTyped/DefinitelyTyped | node-notifier | [#74675](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/74675) | MED |

All PRs were submitted from the `ostec-io` fork to the upstream repository, following each project's contributing conventions as captured in Sprint 1.
