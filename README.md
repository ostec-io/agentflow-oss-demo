# AgentFlow OSS Contribution Pipeline

**AgentFlow autonomously contributed to four of the largest open-source repositories on GitHub — microsoft/vscode, kubernetes/kubernetes, DefinitelyTyped/DefinitelyTyped, and grafana/grafana — across two pipeline generations.**

It forked the repos, read each project's contribution guidelines, scouted and selected real issues, implemented fixes with tests, ran build/lint/validation gates, and submitted 14 pull requests following each project's specific rules.

Total human intervention: approving which issues to work on. Everything else was autonomous.

---

## The Numbers

| Metric | V1 | V2 | Total |
|--------|-----|-----|-------|
| Repositories | 3 | 4 | 4 |
| Languages | TypeScript, Go | TypeScript, Go | TypeScript, Go |
| Issues Fixed | 6 | 8 | 14 |
| Pull Requests Submitted | 6 | 8 | 14 |
| Pipeline Phases | 4 | 6 | — |
| Total AgentFlow Tasks | 30+ | 50+ | 80+ |
| Human Decision Points | 1 | 1 | 2 |
| Approximate Wall Clock | ~2 hours | ~1.5 hours | ~3.5 hours |

---

## The Pull Requests

Every PR below is real, submitted to the upstream repository, and follows that project's contribution rules.

### V1 Pipeline (March 11, 2026)

| Repository | Difficulty | Issue | PR |
|------------|-----------|-------|-----|
| microsoft/vscode | LOW | [#170868](https://github.com/microsoft/vscode/issues/170868) — Adapt bash for BusyBox | [PR #300900](https://github.com/microsoft/vscode/pull/300900) |
| microsoft/vscode | MEDIUM | [#141031](https://github.com/microsoft/vscode/issues/141031) — x64 auto-update error on ARM64 Windows | [PR #300902](https://github.com/microsoft/vscode/pull/300902) |
| kubernetes/kubernetes | LOW | [#137520](https://github.com/kubernetes/kubernetes/issues/137520) — Missing sample-device-plugin:1.7 in test manifest | [PR #137644](https://github.com/kubernetes/kubernetes/pull/137644) |
| kubernetes/kubernetes | MEDIUM | [#93474](https://github.com/kubernetes/kubernetes/issues/93474) — kubectl --request-timeout breaks in-cluster config | [PR #137645](https://github.com/kubernetes/kubernetes/pull/137645) |
| DefinitelyTyped | LOW | scheduler — Missing 0.27 exports | [PR #74674](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/74674) |
| DefinitelyTyped | MEDIUM | node-notifier — v10 type surface alignment | [PR #74675](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/74675) |

### V2 Pipeline (March 12, 2026)

| Repository | Difficulty | Issue | PR |
|------------|-----------|-------|-----|
| microsoft/vscode | LOW | [#123786](https://github.com/microsoft/vscode/issues/123786) — Config enum icons | [PR #301136](https://github.com/microsoft/vscode/pull/301136) |
| microsoft/vscode | MEDIUM | [#167266](https://github.com/microsoft/vscode/issues/167266) — X11 primary selection buffer | [PR #301137](https://github.com/microsoft/vscode/pull/301137) |
| kubernetes/kubernetes | LOW | [#131986](https://github.com/kubernetes/kubernetes/issues/131986) — Kubelet metrics docs broken link | [website PR #54882](https://github.com/kubernetes/website/pull/54882) |
| kubernetes/kubernetes | MEDIUM | [#129953](https://github.com/kubernetes/kubernetes/issues/129953) — Flaky kubetest.diffResources | [PR #137668](https://github.com/kubernetes/kubernetes/pull/137668) |
| DefinitelyTyped | LOW | [#74157](https://github.com/DefinitelyTyped/DefinitelyTyped/issues/74157) — dom-webcodecs BufferSource | [PR #74679](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/74679) |
| DefinitelyTyped | MEDIUM | [#74664](https://github.com/DefinitelyTyped/DefinitelyTyped/issues/74664) — React commandFor/command | [PR #74680](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/74680) |
| grafana/grafana | LOW | [#118633](https://github.com/grafana/grafana/issues/118633) — Email remote content loading | [PR #120201](https://github.com/grafana/grafana/pull/120201) |
| grafana/grafana | MEDIUM | [#118097](https://github.com/grafana/grafana/issues/118097) — CloudWatch log group pagination | [PR #120203](https://github.com/grafana/grafana/pull/120203) |

---

## Pipeline Architecture

### V1 Pipeline (4 phases)

The original pipeline ran 4 sequential phases with 3-task fix tracks.

### V2 Pipeline (6 phases)

V2 added a prerequisites gate, repo-specific formatting checks, config validation, and post-submission monitoring. The fix pipeline expanded from 3 to 5 tasks per issue.

```
PHASE 0: Prerequisites Gate (NEW in V2)
  CLA/DCO status, fork health, CI pre-flight
  ► GATE: Any FAIL → human task, pipeline pauses
       │
       ▼
PHASE 1: Setup + Documentation
  Fork repos, read CONTRIBUTING.md, labels, CI, PR templates
  Store structured memory: contributing guide, formatters, CI checks, PR template, bot inventory
       │
       ▼
PHASE 2: Issue Scouting + Selection
  Search GitHub issues by label, score by difficulty rubric
  Select 1 LOW + 1 MEDIUM per repo
  ► HUMAN GATE: Review and approve selections
       │
       ▼
PHASE 3: Fix Implementation (8 parallel tracks in V2)
  Each track runs a 5-task sequential pipeline:
  ┌──────────────┐   ┌────────────┐   ┌────────────┐   ┌───────────────┐   ┌────────────┐
  │ Branch + Code│ → │ Format     │ → │ Build/Lint  │ → │ Config        │ → │ Validation │
  │ Implementation│  │ Check (V2) │   │ Gate        │   │ Validation(V2)│   │ Gate       │
  └──────────────┘   └────────────┘   └────────────┘   └───────────────┘   └────────────┘
       │
       ▼
PHASE 4: PR Submission (8 parallel in V2)
  Create PRs following repo-specific rules
  Include release-note blocks, labels, reviewer suggestions
  Store PR URLs in project memory
       │
       ▼
PHASE 5: Post-Submission Monitoring (NEW in V2)
  Scheduled sweep every 4 hours via cron trigger
  Auto-fix formatting failures, create human tasks for reviewer questions
  Track PR status: open → merged/closed
```

---

## What Each Phase Did

### V1 Details

See the V1 docs for the original pipeline execution:
- [Phase 1: Setup](docs/sprint-1-setup.md)
- [Phase 2: Scouting](docs/sprint-2-scouting.md)
- [Phase 3: Fixes](docs/sprint-3-fixes.md)
- [Phase 4: PRs](docs/sprint-4-prs.md)

### V2 Improvements

**Phase 0 (Prerequisites Gate)** — CLA/DCO verification, fork health checks, and CI pre-flight. Caught the CLA gap that blocked both K8s v1 PRs. For v2, all 4 repos passed prerequisites before any code work began.

**Phase 1 (Structured Memory)** — Expanded from a single contributing guide to 5 structured memory topics per repo: `contributing-guide`, `formatters`, `ci-checks`, `pr-template`, `bot-inventory`. This gave downstream agents explicit knowledge of each repo's formatting tools, required PR body fields, and bot enforcement rules.

**Phase 3 (5-Task Pipeline)** — Added two new validation steps:
- **Format Check** — Ran each repo's specific formatter (dprint for DT, gofmt for K8s, ESLint for VS Code, prettier for Grafana)
- **Config Validation** — Verified config files (tsconfig.json, go.mod, etc.) against canonical templates

**Phase 4 (PR Body Generation)** — Used structured memory to generate repo-specific PR body fields: release-note blocks for K8s, type annotations for Grafana, proper title format for DT.

**Phase 5 (Monitoring)** — Cron triggers (every 4 hours) sweep all active PRs for new comments, CI failures, and reviewer feedback. Bot formatting failures are auto-fixed. Reviewer questions create human tasks for review before responding.

### V2 Performance

| Repo | Build | Lint | Formatter | Branch |
|------|-------|------|-----------|--------|
| VS Code | `npm run compile` | ESLint | ESLint --fix | `fix/<issue>-<desc>` |
| Kubernetes | `go build` / `go test` | `gofmt`, `goimports` | gofmt | `fix/issue-<number>-<desc>` |
| DefinitelyTyped | `npx dtslint types/<pkg>` | dtslint | dprint | `types/<pkg>-fix-<desc>` |
| Grafana | `make build-go` | `golangci-lint` | prettier, gofmt | `fix/<issue>-<desc>` |

V2 used Claude agent type instead of Codex, reducing per-task execution time by 3-10x (2-7 min vs 7-39 min).

---

## The Fixes

### V1 Fixes

| Issue | Summary | Files |
|-------|---------|-------|
| VS Code #170868 | BusyBox bash guard — early-return when `/bin/bash` is ash | 1 |
| VS Code #141031 | ARM64 Windows auto-update detection + banner | 6+ |
| K8s #137520 | Missing sample-device-plugin:1.7 image manifest | 2 |
| K8s #93474 | kubectl --request-timeout in-cluster config fix | 4 |
| DT scheduler | Missing 0.27 exports + subpath entry points | 6 |
| DT node-notifier | v10 type surface alignment + EventEmitter inheritance | 7 |

### V2 Fixes

| Issue | Summary | Files |
|-------|---------|-------|
| VS Code #123786 | Config enum icon support in settings UI | TBD |
| VS Code #167266 | X11 primary selection buffer support in inputs | TBD |
| K8s #131986 | Fix broken Summary API link in kubelet metrics docs | 1-2 |
| K8s #129953 | Fix flaky kubetest.diffResources test | TBD |
| DT #74157 | dom-webcodecs BufferSource type alignment | TBD |
| DT #74664 | React commandFor/command prop types | TBD |
| Grafana #118633 | Email notification remote content loading | TBD |
| Grafana #118097 | CloudWatch log group pagination fix | TBD |

**Full fix details:** [results/](results/)

---

## Human Involvement

| Step | V1 | V2 |
|------|-----|-----|
| Initial instruction | "Fix bugs in these 3 repos" | "Run the v2 pipeline on all 4 repos" |
| Issue selection review | Approved 6 issues | Approved 8 issues |
| Recovery actions | Retry without branchIsolation | None needed |
| Status checks | Asked for updates | Asked for updates |

Both pipeline runs had a single human gate (issue selection). V2 required no recovery actions — the prerequisites gate and structured memory eliminated the issues that caused v1 retries.

---

## AgentFlow Features Used

- **Projects** — 4 projects with GitHub repos attached
- **Sprints** — Sequential and parallel execution modes
- **Tasks** — Draft creation, template instantiation, sequential pipeline
- **Task Templates** — 8 custom templates (prerequisites, contribution guide, issue scout, format check, config validation, PR submitter, PR monitor, generic task)
- **Triggers** — Cron triggers for post-submission monitoring (every 4h)
- **Project Memory** — 5 structured topics per repo (contributing guide, formatters, CI checks, PR template, bot inventory)
- **MCP Tools** — 386+ AgentFlow tools exposed via Model Context Protocol
- **Agent Types** — V1 used Codex agents; V2 switched to Claude agents for 3-10x faster execution

---

## V2 Lessons Learned

Building on [V1 lessons](docs/lessons-learned.md):

- **Prerequisites gates prevent wasted work** — V1's unsigned CLA blocked both K8s PRs post-submission. V2 catches this before any code work begins.
- **Structured memory > flat guides** — Breaking contributing docs into 5 typed topics (formatters, CI, PR template, bots, guide) lets downstream agents query exactly what they need.
- **Format checks as a separate task** — Repo-specific formatters (dprint, gofmt, prettier) caught issues that generic linters missed.
- **Claude agent type is significantly faster** — 2-7 min per task vs 7-39 min with Codex, with no quality loss.
- **Combined validation + PR submission** — V2 consolidated 3 validation tasks + PR submission into a single task, reducing overhead without losing quality.
- **Post-submission monitoring is essential** — V1 had no visibility into bot comments or reviewer feedback after PR submission. V2's cron-triggered sweep catches issues within 4 hours.

---

## About AgentFlow

[AgentFlow](https://agentflow.ostec.io) is a cognitive orchestration platform that manages AI agent sessions, context, and compliance at scale. Built by [OSTEC](https://ostec.io).

---

*V1 executed March 11, 2026. V2 executed March 12, 2026.*
