# Decisions

Append-only log. A superseded decision stays with its number and a
`superseded-by` note; never edit history in place.

## Format

```
## DNN — short title (YYYY-MM-DD)
Status: active | superseded-by-DNN
Decision. Context. Consequences.
```

## D01 — mi-lana agent scaffolding (2026-11-09)

Status: active

Repo adopts the mi-lana agent setup verbatim: `.opencode/opencode.jsonc` with
orchestrator (default primary), locator, builder and reviewer agents,
deny-by-default permissions, env reads denied, same opencode-go model roster.
Delegation briefs in megacave (Classical Chinese 文言文); reports in megacave;
user-facing relay in caveman.

Consequences: anyone cloning the repo gets identical agent behavior. The
migration agent is intentionally absent until a database exists.

## D02 — stack deferred (2026-11-09)

Status: active

No stack chosen yet. Prompts and `AGENTS.md` carry `TBD (stack)` placeholders.
First implementation plan must record the stack, its invariants and its
commands in `AGENTS.md` and here.
