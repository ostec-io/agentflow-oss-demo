# Sprint 1: Setup + Documentation Ingestion

## Projects Created

| Project | AgentFlow ID |
|---|---|
| OSS - VS Code | `69b1acd85dd9b645b667146b` |
| OSS - Kubernetes | `69b1acd95dd9b645b667146c` |
| OSS - DefinitelyTyped | `69b1acda5dd9b645b667146d` |

## Repository Forks

All three repositories were forked into the `ostec-io` GitHub organization:

```bash
gh repo fork microsoft/vscode --org ostec-io
gh repo fork kubernetes/kubernetes --org ostec-io
gh repo fork DefinitelyTyped/DefinitelyTyped --org ostec-io
```

## Documentation Agents

Agents were deployed to each fork to read and synthesize contributing guidelines. Each agent ingested:

- `CONTRIBUTING.md`
- `.github/` directory (issue templates, PR templates, workflows)
- Label schemas
- CI/CD configuration

The output was a structured contributing guide stored as project memory, making it available to all downstream agents in later sprints.

## Key Findings

### VS Code

- **Package manager:** npm (not yarn)
- **CLA:** Microsoft CLA required
- **Build:** `npm run compile` for quick checks, `npm run compile-check-ts-native` for full typecheck
- **PR format:** `fix: <description> (fixes #<number>)`
- **Reviewers:** Not manually assigned; team triage handles routing

### Kubernetes

- **CI system:** Prow + Tide
- **CLA:** CNCF CLA required
- **Build:** `go test`, `hack/verify-*.sh` scripts
- **PR format:** What/Why/How body structure with `Fixes #<number>`
- **Post-PR:** `/sig <name>` and `/kind <type>` prow bot commands required
- **Commits:** Must include `Signed-off-by` line (DCO)

### DefinitelyTyped

- **Package manager:** pnpm
- **Validation:** dtslint per-package
- **PR format:** `[package-name] description` title
- **Review:** dt-mergebot handles auto-review
- **Commits:** DCO sign-off required
