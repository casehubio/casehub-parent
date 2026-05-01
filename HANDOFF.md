# HANDOFF — CaseHub CI/CD Chain and Rename Cleanup
2026-05-01

## What was done this session

**Major rename completed:** All casehub repos moved to `~/claude/casehub/` with short GitHub repo names (`casehubio/ledger`, `casehubio/work`, etc.). Maven coordinates, Java packages, config prefixes all updated. Stale references cleaned across all docs and CLAUDE.md files.

**CI/CD chain wired:** All 7 repos now have consistent publish workflows:
- Build+test always (including PRs)
- Publish only on non-PR events (`github.event_name != 'pull_request'`)
- `repository_dispatch` chain: parent→ledger,connectors→work,qhorus→engine→claudony

**GitHub Packages unblocked:** 69 stale 0.2-SNAPSHOT packages cleared via `clear-snapshot-packages.yml`. Root cause was 3 layered bugs — classic PAT required (not fine-grained), set-e command substitution, last-version 400 error.

## Current CI state

| Repo | Status | Notes |
|------|--------|-------|
| parent | ✅ | |
| ledger | ✅ | |
| connectors | ✅ | |
| engine | ✅ | |
| claudony | ✅ | |
| work | ❌ | Can't resolve `io.casehub:casehub-parent:pom:0.2-SNAPSHOT` |
| qhorus | ❌ | Same — missing casehub-parent in GitHub Packages |

## Immediate next action

Trigger parent publish (gets cleared by snapshot cleaner), then re-run work and qhorus:

```bash
gh workflow run publish.yml --repo casehubio/parent
# Wait ~3 min, then:
gh workflow run publish.yml --repo casehubio/work
gh workflow run publish.yml --repo casehubio/qhorus
```

## References

| Item | Location |
|------|----------|
| Platform architecture | `docs/PLATFORM.md` |
| Foundation roadmap epic | https://github.com/casehubio/parent/issues/7 |
| Ledger trust epic | https://github.com/casehubio/ledger/issues/48 |
| Clear-snapshot workflow | `.github/workflows/clear-snapshot-packages.yml` |
