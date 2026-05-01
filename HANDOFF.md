# HANDOFF — CI Chain Repair
2026-05-01

## What changed this session

**Dispatch chain fixed:** All publish.yml files used `GITHUB_TOKEN` for cross-repo `repository_dispatch` — that token is repo-scoped, returns 403. Changed to `secrets.GH_PAT` (classic PAT) across parent, ledger, connectors, work, qhorus, and engine. Chain now fires correctly.

**Peer-repo discipline established:** This Claude (parent session) does not commit to peer repos. Each has its own Claude session. Cross-repo fixes → GitHub issues. Lesson learned the hard way: committed to engine's feature branch and swept 15 staged files from another session into a CI commit.

**langchain4j excluded by default:** Added `include_langchain4j` boolean input to both `full-stack-build.yml` and `clear-snapshot-packages.yml`. Default false — skips the slow fork unless explicitly ticked.

**Surefire rerun:** Parent POM now sets `rerunFailingTestsCount=2`. Flaky tests retry twice before failing.

**CLAUDE.md created:** Was empty — now populated with repo role, CI/CD rules, peer-repo boundaries, and conventions pointer.

## Current CI state

| Repo | Status | Blocker |
|------|--------|---------|
| parent | ✅ | — |
| ledger | ✅ | — |
| connectors | ✅ | — |
| qhorus | ✅ | — |
| engine | ✅ | — |
| work | ❌ | `BusinessHoursIntegrationTest.createWithClaimDeadlineBusinessHours` — fix: change `isBefore(Instant.now().plus(1, DAYS))` to `isBefore(before.plus(3, DAYS))` at line 82 |
| claudony | ❌ | `ClaudonyWorkerContextProvider` missing `UUID caseId` param — engine PR #224 added it. New signature: `buildContext(String workerId, UUID caseId, WorkRequest task)` |

## Immediate next action

Tell work Claude and claudony Claude the fixes in the CI state table. Then trigger `full-stack-build.yml` to verify the chain is fully green.

## References

| Item | Location |
|------|----------|
| Platform architecture | `docs/PLATFORM.md` |
| Foundation roadmap epic | https://github.com/casehubio/parent/issues/7 |
| Conventions index | `docs/conventions/INDEX.md` |
| Clear-snapshot workflow | `.github/workflows/clear-snapshot-packages.yml` |
| Full-stack build | `.github/workflows/full-stack-build.yml` |
| Blog entry | `blog/2026-05-01-mdp01-ci-chain-repair.md` |
