# 75 Hard Challenge — OpenCode agent setup

Portable agent configuration: one orchestrator that plans and delegates, three
subagents with narrow write scopes. Copied from the mi-lana setup; copy this
directory into another repo to reuse it.

```
.opencode/
├── opencode.jsonc      # agents, models, permissions
└── prompts/
    ├── orchestrator.md # plan → approval gate → delegate
    ├── locator.md      # read-only code search
    ├── builder.md      # edits src/, e2e/, plan checkboxes
    └── reviewer.md     # read-only diff review
```

## How it works

1. `orchestrator` is the default primary agent. It reads the repo docs, writes a
   numbered plan under `docs/plans/`, and stops for explicit approval.
2. After approval it delegates independent units, integrates the results, and
   keeps the plan checklist current.
3. Subagents cannot ask questions. A blocked subagent reports back instead of
   guessing.

Permissions are deny-by-default per role:

| Agent | Mode | Writes | Shell |
| --- | --- | --- | --- |
| `orchestrator` | primary | anything, small diffs | `git push`, `git reset --hard`, `git clean` ask |
| `locator` | subagent | nothing | denied |
| `builder` | subagent | `src/**`, `docs/plans/**`, `e2e/**` | push/rm/sudo denied |
| `reviewer` | subagent | nothing | only `git diff` |

Every agent denies reads of `*.env` and `*.env.*` (`*.env.example` is allowed).

## Model routing

| Agent | Model | Why |
| --- | --- | --- |
| default `model` | `opencode-go/glm-5.3` | root default cannot keep a `#variant` |
| `orchestrator` | `opencode-go/glm-5.3#high` | planning and integration carry the most ambiguity |
| `locator` | `opencode-go/glm-5.3-flash#high` | bounded search, cheap and fast |
| `builder` | `opencode-go/glm-5.3-flash#high` | scoped edits with a clear spec |
| `reviewer` | `opencode-go/gpt-6-luna#high` | catches mistakes the builder model makes |
| `explore` (built-in) | `opencode-go/qwen3.8-flash#low` | broad orientation on its own allowance |
| `general` (built-in) | `opencode-go/minimax-m3.1-flash#medium` | parallel research tasks; orchestrator denies launch |
| `title` (built-in) | `opencode-go/mimo-v2.6-flash` | session titles and other low-stakes calls |

IDs are OpenCode Go models (`opencode-go/<model-id>`). `#variant` is valid on
agent entries only. The root `model` drops it. Do not use Muse Spark or Iceman
Contributor here: those endpoints train on request data.

## Install in another repo

```bash
cp -R .opencode /path/to/your-repo/
```

Then:

1. Edit `opencode.jsonc` and replace model IDs with ones your provider exposes.
2. Replace the repo-specific invariants inside `prompts/*.md` with your own.
   Keep the reporting and approval-gate lines.
3. Run `opencode` in the repo root. `orchestrator` loads by default.
4. Keep the config in version control so every teammate and every session
   starts from the same permissions.

Requires OpenCode with agent definitions in `opencode.jsonc` and
`{file:...}` prompt substitution. Verify both resolve before relying on the
approval gate; it is prompt-level, not enforced by the runtime.

## Design notes

- Plans are written by the agent, approved by a human, then frozen. Only
  checkboxes and status change during execution.
- `decisions.md` is append-only. Changed decisions are marked superseded, never
  edited.
- One model per role beats one model for everything: expensive reasoning where
  a wrong call is costly, cheap models where the scope is already bounded.
- The orchestrator denies `general` on purpose: a generic subagent would bypass
  the narrow write scopes above. Delegation goes to a named role or nowhere.
- No `migration` agent: this repo has no database yet. When one lands, copy the
  migration agent (prompt + permissions) from mi-lana.
