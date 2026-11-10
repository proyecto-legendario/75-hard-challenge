---
plan: 001-provisioning
status: done
created: 2026-11-09
scope: Provision this repo with the mi-lana agent setup (opencode settings, prompts, permissions) and the same documentation structure, so anyone cloning it works with identical agent behavior.
---

# Plan 001 — provisioning

## Goal

Clone the mi-lana agent scaffolding into this empty repo: `.opencode/` (agents,
models, permissions, prompts), `AGENTS.md`, `README.md`, and the living docs
under `docs/`. Stack is TBD, so prompts carry placeholder invariants instead of
Next.js/Supabase rules. Anyone who clones the repo gets the same setup with no
manual steps.

## Owner decisions

- Stack TBD: prompts stay stack-agnostic. Each prompt keeps a "Repo
  invariants" section with placeholders marked `TBD (stack)`. The migration
  agent is dropped until a database exists; re-add it from mi-lana when one
  lands.
- UI language TBD: the builder prompt keeps a placeholder line for the UI
  language convention; fill in when decided.
- Delegation register stays as mi-lana: delegation briefs in megacave
  (Classical Chinese 文言文), subagent reports in megacave, relay to the user
  in caveman.
- Model roster reused verbatim from mi-lana (`opencode-go/...` IDs, variants
  on agent entries only).
- Permissions copied verbatim: deny-by-default per role, env reads denied
  (`*.env.example` allowed), orchestrator asks before `git push` /
  `git reset --hard` / `git clean`.
- Builder write scope without a `src/` tree yet: allow `src/**`,
  `docs/plans/**`, `e2e/**` (created later) — same as mi-lana; add scopes
  when the stack lands.

## Commit checklist

Items describe logical work units; commits require a separate user request.

- [x] 1. `chore: add opencode agent configuration` — copy `.opencode/opencode.jsonc`, `.opencode/.gitignore`, `.opencode/README.md` from mi-lana; drop the migration agent; keep model roster and permissions.
- [x] 2. `docs: add agent prompts` — adapt the five mi-lana prompts (orchestrator, locator, builder, migration removed, reviewer) with stack-TBD placeholder invariants; keep megacave/caveman register and the approval-gate lines.
- [x] 3. `docs: add repo guide and living docs skeleton` — `AGENTS.md` (commands, commit conventions, plan versioning), `README.md` (project intro, agent model), `docs/decisions.md`, `docs/use-cases.md`, `docs/user-flows.md`, `docs/known-limitations.md`.
- [x] 4. `docs: record provisioning plan` — mark this plan done, verify every prompt referenced by `opencode.jsonc` resolves (`{file:...}` paths), and confirm the orchestrator loads. Verified 2026-11-09: config parses (comment-stripped JSON), all four `{file:...}` prompts exist, `default_agent: orchestrator`, model `opencode-go/glm-5.3`. Fresh session loads the new config; this session predates it.

## References

- Source: `../mi-lana/.opencode/` and `../mi-lana/AGENTS.md`
