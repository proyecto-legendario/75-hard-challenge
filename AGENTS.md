# 75 Hard Challenge — agent guide

Progress tracker for the 75 Hard challenge. Stack TBD: until the first
implementation plan records it in `docs/decisions.md`, treat all stack rules
as placeholders.

**Active plan:** [`docs/plans/001-provisioning.md`](./docs/plans/001-provisioning.md) — done. No plan in progress; the next plan takes `002-`.

## Commands

TBD (stack). Record the dev/build/lint commands here when the stack lands.

## Commit conventions

- **Conventional Commits**: `feat:`, `fix:`, `chore:`, `docs:` (scopes optional, e.g. `feat(ui):`)
- **Atomic commits**: one logical change per commit; the app must work at every commit
- **Branches**: `main` is the integration branch. All new work happens on feature branches (`feat/...`, `fix/...`, `chore/...`) and merges back to `main` with `--no-ff`
- **Push everything**: push feature branches to `origin` when work lands (not just `main`) so all work is visible on GitHub; keep branches after merge for history
- **Plan checkboxes**: tick `docs/plans/NNN-*.md` items in the same commit as the work they describe
- **Rollback**: prefer `git revert <sha>`; never rewrite pushed history
- Milestones get tags (e.g. `v0.1-mvp`)

## Plan versioning (docs/plans/)

- Each iteration gets a numbered file: `docs/plans/NNN-slug.md` (`001-mvp.md`, `002-....md`)
- Header carries `status: proposed | in-progress | done | superseded-by-NNN`
- Plans contain a **commit checklist** — check items off as they land; a new session resumes from the first unchecked item
- Plans are **frozen** once execution starts (only checkboxes/status change)
- **Living docs** (always current, edit freely): `docs/decisions.md`, `docs/use-cases.md`, `docs/user-flows.md`, `docs/known-limitations.md`, `README.md`, this file
- When a plan completes: mark `done`, fold outcomes into living docs, point "Active plan" above at the next file

## Agent setup

See [`.opencode/README.md`](./.opencode/README.md). One orchestrator (default
primary) plans and delegates; `locator`, `builder` and `reviewer` subagents
have narrow, deny-by-default write scopes. The orchestrator writes a numbered
plan under `docs/plans/` and stops for explicit approval before any
implementation work.

## Architecture

TBD (stack). Record the architecture here when the first implementation plan lands.
