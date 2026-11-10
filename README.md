# 75 Hard Challenge

Progress tracker for the 75 Hard challenge. Stack TBD.

## How agents work in this repo

- `opencode` starts with the **orchestrator** as the default primary agent. It
  reads the docs, drafts a numbered plan in `docs/plans/`, and stops for your
  explicit approval before any implementation work.
- After approval it delegates to subagents with narrow write scopes
  (`locator` read-only search, `builder` edits `src/**` + plan checkboxes,
  `reviewer` read-only diff review) and keeps the plan checklist in sync.
- Every agent denies reads of `*.env` files. The orchestrator asks before
  `git push`, `git reset --hard` and `git clean`.
- Full setup and portability notes: [`.opencode/README.md`](./.opencode/README.md).
- Repo rules for agents: [`AGENTS.md`](./AGENTS.md).

## Documentation layout

| Path | Kind |
| --- | --- |
| `docs/plans/` | numbered, frozen plans with commit checklists |
| `docs/decisions.md` | append-only decision log |
| `docs/use-cases.md` | living doc |
| `docs/user-flows.md` | living doc |
| `docs/known-limitations.md` | living doc |
