# Lessons Learned

## 1. Branch Isolation Fails on Large Repositories

The sidecar's `prepare-task-branch` step times out on repositories the size of vscode (~700k commits) and kubernetes (~120k commits). The `branchIsolation=true` flag triggers a full clone preparation that exceeds the context deadline.

**Workaround:** Disable `branchIsolation` and handle branching with direct git commands inside the task. This gives agents full control over shallow clones and fetch depth.

**Future fix:** The sidecar should support configurable timeouts or shallow-clone strategies for branch isolation on large repos.

## 2. Parallel Execution Works Well for Independent Fix Tracks

Running 6 sprints in parallel (one per issue) had no conflicts or resource contention. Each agent operated on its own fork branch. This pattern is ideal when fixes are independent and don't touch overlapping files.

## 3. Project Memory Is Critical for Multi-Sprint Pipelines

Contributing guides, fix plans, and build results were stored as project memory in Sprint 1 and consumed by agents in Sprints 2-4. Without this shared context:

- Agents would not know the correct build commands
- PR formatting would not match upstream conventions
- Prow commands for Kubernetes would be missed

Project memory eliminated redundant documentation reads and ensured consistency across all agents working on the same repository.

## 4. Different Repos Need Different CI Validation

Each repository has its own build system, linter, and test framework. Agents adapted automatically based on the contributing guides ingested in Sprint 1:

- **VS Code:** `npm run compile` + TypeScript typecheck
- **Kubernetes:** `go test` + `hack/verify-*.sh` shell scripts
- **DefinitelyTyped:** `dtslint` per-package validation

No hardcoded CI logic was needed -- agents derived the correct validation steps from project memory.

## 5. A Single Human Gate Is Sufficient

The only manual intervention was approving the 6 issue selections after Sprint 2. Everything before (documentation ingestion, scouting) and after (implementation, validation, PR submission) ran autonomously. This confirms that the bottleneck in autonomous OSS contribution is judgment about *what* to fix, not *how* to fix it.
