# AgentFlow OSS Contribution Pipeline

**AgentFlow autonomously contributed to three of the largest open-source repositories on GitHub — microsoft/vscode, kubernetes/kubernetes, and DefinitelyTyped/DefinitelyTyped — in a single session.**

It forked the repos, read each project's contribution guidelines, scouted and selected real issues, implemented fixes with tests, ran build/lint/validation gates, and submitted 6 pull requests following each project's specific rules.

Total human intervention: approving which issues to work on. Everything else was autonomous.

---

## The Numbers

| Metric | Value |
|--------|-------|
| Repositories | 3 (vscode, kubernetes, DefinitelyTyped) |
| Languages | TypeScript, Go |
| Issues Fixed | 6 (1 LOW + 1 MEDIUM per repo) |
| Pull Requests Submitted | 6 |
| Pipeline Phases | 4 sprints |
| Total AgentFlow Tasks | 30+ |
| Human Decision Points | 1 (issue selection approval) |
| Approximate Wall Clock | ~2 hours |

---

## The Pull Requests

Every PR below is real, submitted to the upstream repository, and follows that project's contribution rules.

| Repository | Difficulty | Issue | PR |
|------------|-----------|-------|-----|
| microsoft/vscode | LOW | [#170868](https://github.com/microsoft/vscode/issues/170868) — Adapt bash for BusyBox | [PR #300900](https://github.com/microsoft/vscode/pull/300900) |
| microsoft/vscode | MEDIUM | [#141031](https://github.com/microsoft/vscode/issues/141031) — x64 auto-update error on ARM64 Windows | [PR #300902](https://github.com/microsoft/vscode/pull/300902) |
| kubernetes/kubernetes | LOW | [#137520](https://github.com/kubernetes/kubernetes/issues/137520) — Missing sample-device-plugin:1.7 in test manifest | [PR #137644](https://github.com/kubernetes/kubernetes/pull/137644) |
| kubernetes/kubernetes | MEDIUM | [#93474](https://github.com/kubernetes/kubernetes/issues/93474) — kubectl --request-timeout breaks in-cluster config | [PR #137645](https://github.com/kubernetes/kubernetes/pull/137645) |
| DefinitelyTyped | LOW | scheduler — Missing 0.27 exports | [PR #74674](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/74674) |
| DefinitelyTyped | MEDIUM | node-notifier — v10 type surface alignment | [PR #74675](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/74675) |

---

## Pipeline Architecture

The pipeline runs in 4 sequential phases, each composed of AgentFlow sprints with autonomous task execution.

```
PHASE 1: Setup + Documentation
  Fork repos into ostec-io org (3 parallel)
  Read CONTRIBUTING.md, labels, CI, PR templates
  Store contributing guides as project memory
       │
       ▼
PHASE 2: Issue Scouting + Selection
  Search GitHub issues by label (good first issue, help wanted, triage/accepted)
  Score by difficulty rubric (files, architecture, tests)
  Select 1 LOW + 1 MEDIUM per repo
  ► HUMAN GATE: Review and approve selections
       │
       ▼
PHASE 3: Fix Implementation (6 parallel tracks)
  Each track runs a 3-task sequential pipeline:
  ┌─────────────────┐    ┌────────────────┐    ┌─────────────────┐
  │ Branch + Code   │ →  │ Build/Lint Gate │ →  │ Validation Gate │
  │ Implementation  │    │ (iterative fix) │    │ (acceptance)    │
  └─────────────────┘    └────────────────┘    └─────────────────┘
       │
       ▼
PHASE 4: PR Submission (6 parallel)
  Push branches to forks
  Create PRs following repo-specific rules
  Post prow commands (Kubernetes)
  Store PR URLs in project memory
```

---

## What Each Phase Did

### Phase 1: Setup + Documentation

AgentFlow created 3 projects, forked each repository into the `ostec-io` GitHub organisation, and deployed agents to read each repo's contribution documentation. Each agent produced a structured contributing guide covering:

- How to find issues (labels, search queries)
- PR process and review requirements
- CI/CD pipeline and local validation commands
- Coding conventions and commit format
- Workflow-specific notes (CLA, DCO, prow commands, dt-mergebot)

These guides were stored as **project memory entries** in AgentFlow, making them available to all subsequent agents working on that project.

**Details:** [docs/sprint-1-setup.md](docs/sprint-1-setup.md)

### Phase 2: Issue Scouting

Each project got a scouting agent that searched GitHub for contribution-ready issues and scored them using a difficulty rubric:

| Factor | LOW | MEDIUM |
|--------|-----|--------|
| Files touched | ≤2 | 3–5 |
| Architecture knowledge | Self-contained | One subsystem |
| Test complexity | Existing pattern | Non-trivial additions |

The agents selected 2 issues per project (1 LOW + 1 MEDIUM), created AgentFlow issues for each, and stored their reasoning in project memory.

This was the **only human gate** in the pipeline — I reviewed the 6 selections before proceeding. In a fully autonomous configuration, this gate can be removed.

**Details:** [docs/sprint-2-scouting.md](docs/sprint-2-scouting.md)

### Phase 3: Fix Implementation

Six fix tracks ran in parallel, each as its own AgentFlow sprint with a 3-task sequential pipeline:

1. **Branch + Implement** — Clone fork, create branch, recall fix plan from memory, implement fix with tests, commit, push
2. **Build/Lint Gate** — Run repo-specific build/lint commands, fix any failures iteratively
3. **Validation Gate** — Verify all acceptance criteria against the fix plan

**Repo-specific conventions followed:**

| Repo | Build | Lint | Commit | Branch |
|------|-------|------|--------|--------|
| VS Code | `npm run compile` | ESLint on changed files | Standard | `fix/<issue>-<desc>` |
| Kubernetes | `go build` / `go test` | `gofmt`, `goimports` | `Signed-off-by` via `git commit -s` | `fix/issue-<number>-<desc>` |
| DefinitelyTyped | `npx dtslint types/<pkg>` | dtslint | `[pkg] desc` + DCO sign-off | `types/<pkg>-fix-<desc>` |

**The recovery story:** The first Sprint 3 attempt used AgentFlow's `branchIsolation` feature, which creates isolated git branches via the sidecar. On repositories this large (vscode ~2GB, kubernetes ~3GB), the sidecar's `prepare-task-branch` endpoint timed out. All 6 sprints failed at the branch setup task. The fix: recreate the sprints without `branchIsolation`, using direct `git checkout -b` commands in the implementation task instead. All 6 retried successfully.

**Details:** [docs/sprint-3-fixes.md](docs/sprint-3-fixes.md)

### Phase 4: PR Submission

Six PR submission agents ran in parallel, each following the target repository's specific PR conventions:

- **VS Code:** Title format `fix: <description> (fixes #<number>)`, no manual reviewer assignment
- **Kubernetes:** PR body with What/Why/How sections, `Fixes #<number>`, post-create prow comments (`/sig <name>`, `/kind bug`)
- **DefinitelyTyped:** Title format `[package-name] description` (bot-enforced), dt-mergebot auto-requests reviewers from OWNERS

All 6 PRs were submitted in under 4 minutes.

**Details:** [docs/sprint-4-prs.md](docs/sprint-4-prs.md)

---

## The Fixes

### VS Code #170868 — BusyBox Bash Guard (LOW)

Added an early-return guard in `shellIntegration-bash.sh` that detects when `/bin/bash` is actually BusyBox ash and gracefully skips shell integration features that rely on bash-specific builtins.

**Files changed:** 1 (`src/vs/workbench/contrib/terminal/common/scripts/shellIntegration-bash.sh`)

### VS Code #141031 — ARM64 Windows Auto-Update (MEDIUM)

Detects when VS Code is running as an x64 build on ARM64 Windows, disables auto-updates for that incompatible architecture, and shows a workbench banner directing users to the ARM64 build.

**Files changed:** 6+ (update service, native host service, update contribution, tooltip, tests)

### Kubernetes #137520 — Sample Device Plugin Image (LOW)

Added the missing `sample-device-plugin:1.7` image entry to `test/utils/image/manifest.go` and updated `.permitted-images` to keep verification scripts in sync.

**Files changed:** 2 (`test/utils/image/manifest.go`, `test/images/.permitted-images`)

### Kubernetes #93474 — kubectl --request-timeout (MEDIUM)

Fixed the client config loading path so that `--request-timeout` (and similar CLI flags) no longer suppress in-cluster configuration fallback. Added `applyConfigOverrides` to replay CLI overrides onto in-cluster configs.

**Files changed:** 4 (`config_flags.go`, `client_config.go`, `client_config_test.go`, `merged_client_builder_test.go`)

### DefinitelyTyped: scheduler (LOW)

Synced `@types/scheduler` to match the 0.27 npm package — added missing exports (`unstable_forceFrameRate`, `unstable_requestPaint`, `unstable_Profiling`), typed subpath entry points, and updated tests.

**Files changed:** 6 (index.d.ts, package.json, tsconfig.json, new subpath .d.ts files, test files)

### DefinitelyTyped: node-notifier (MEDIUM)

Aligned `@types/node-notifier` with the v10.0.1 npm package — added root `Notification` constructor alias, `EventEmitter` inheritance on all 5 notifier classes, missing options across notifiers, and comprehensive test coverage.

**Files changed:** 7 (index.d.ts, 5 notifier .d.ts files, test file)

**Full fix details:** [results/](results/)

---

## Human Involvement

| Step | Human Action | Could Be Autonomous? |
|------|-------------|---------------------|
| Initial instruction | "Fix bugs in these 3 repos" | Required — defines the task |
| Issue selection review | Approved the 6 selected issues | Yes — can be fully autonomous |
| Sprint 3 failure recovery | Said "yes, retry without branchIsolation" | Yes — AgentFlow could auto-retry |
| Status checks | Asked for updates during execution | Optional — monitoring only |

The pipeline was designed with one human gate (issue selection) for this demo. In production, this gate can be removed for fully autonomous operation.

---

## AgentFlow Features Used

- **Projects** — 3 projects with GitHub repos attached
- **Sprints** — Sequential and parallel execution modes
- **Tasks** — Draft creation, sprint assignment, sequential pipeline
- **Task Templates** — 3 custom templates (contribution guide, issue scout, PR submitter)
- **Triggers** — Webhook and system_event triggers for automation
- **Project Memory** — Contributing guides, fix plans, build results, validation reports stored per-project
- **MCP Tools** — 386+ AgentFlow tools exposed via Model Context Protocol

---

## Lessons Learned

See [docs/lessons-learned.md](docs/lessons-learned.md) for detailed notes on:

- Branch isolation vs direct git commands for large repos
- Parallel sprint execution across projects
- Repo-specific CI validation challenges
- The value of project memory for multi-agent workflows

---

## About AgentFlow

[AgentFlow](https://agentflow.ostec.io) is a cognitive orchestration platform that manages AI agent sessions, context, and compliance at scale. Built by [OSTEC](https://ostec.io).

---

*This demo was executed on March 11, 2026.*
