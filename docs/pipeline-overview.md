# Pipeline Overview

**Date:** March 11, 2026

## Objective

Demonstrate AgentFlow's ability to autonomously contribute to major open-source repositories -- from issue scouting through PR submission -- with a single human approval gate.

## Target Repositories

| Repository | Language | Fork Org |
|---|---|---|
| [microsoft/vscode](https://github.com/microsoft/vscode) | TypeScript | ostec-io |
| [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes) | Go | ostec-io |
| [DefinitelyTyped/DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) | TypeScript | ostec-io |

## Pipeline Stages

```
Sprint 1: Setup + Docs  -->  Sprint 2: Issue Scouting  -->  [Human Gate]  -->  Sprint 3: Fixes  -->  Sprint 4: PRs
```

1. **Sprint 1 -- Setup + Documentation Ingestion:** Create AgentFlow projects, fork repos, deploy agents to read contributing guides and CI configs. Output: structured contributing guides stored as project memory.

2. **Sprint 2 -- Issue Scouting:** Agents scan open issues, score candidates on a LOW/MED rubric (files touched, architecture knowledge, test complexity). Output: 2 issues per repo (1 LOW, 1 MED).

3. **Human Gate:** The only manual step. A human reviews and approves the 6 selected issues.

4. **Sprint 3 -- Fix Implementation:** 6 parallel sprints execute a 3-task pipeline per fix: branch + implement, build/lint gate, validation gate. Output: passing commits on fork branches.

5. **Sprint 4 -- PR Submission:** 6 parallel tasks format and submit PRs to upstream repos, following each project's contribution conventions.

## Results

- **6 pull requests** submitted across 3 major OSS projects
- **1 human intervention** (issue selection approval)
- **Total pipeline time:** ~2 hours end-to-end
- All PRs passed local CI validation before submission

## Documentation Index

- [Sprint 1: Setup](sprint-1-setup.md)
- [Sprint 2: Scouting](sprint-2-scouting.md)
- [Sprint 3: Fixes](sprint-3-fixes.md)
- [Sprint 4: PRs](sprint-4-prs.md)
- [Lessons Learned](lessons-learned.md)
- [PR Summary](../results/pr-summary.md)
