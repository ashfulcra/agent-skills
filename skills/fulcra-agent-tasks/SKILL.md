---
name: fulcra-agent-tasks
description: "Give a fulcra-agent-teams space a typed task lifecycle: create tasks with structured status/priority/assignee, and move them through a validated state machine (proposed→active→done) instead of freeform markdown."
homepage: "https://github.com/fulcradynamics/agent-skills"
license: "MIT"
user-invocable: true
metadata: { "openclaw": { "emoji": "✅" } }
---

# Fulcra Agent Tasks

## Installation

This skill uses the `coord-engine` CLI — a small, stdlib-only tool that runs the deterministic folds. Install it once:

```bash
uv tool install "git+https://github.com/ashfulcra/fulcra-tools@coord-engine-v1.3.0#subdirectory=packages/coord-engine"
```

After installation, `coord-engine <command>` is on your PATH.

Enhances the [`fulcra-agent-teams`](https://github.com/fulcradynamics/agent-skills) skill. Bare teams
tracks long-running work as freeform `task/<name>.md`. This skill gives those docs a **typed lifecycle** —
a real `status`/`priority`/`assignee` and a **validated state machine** — so a task's state is queryable
(via the `fulcra-agent-reconcile` skill) and can't take an illegal jump (e.g. `done → active`). Pairs with
`fulcra-agent-reconcile`: this skill *writes* task state; reconcile *reads/heals* the views.

## Why the writes go through the engine
Writing OKF frontmatter correctly and enforcing which transitions are legal are **deterministic**
requirements — a malformed doc or an illegal `waiting → done` is a correctness bug, not a style choice.
So the lifecycle commands are the shared **`coord-engine`** tool (parse→modify→write, transition-checked),
not prose the agent hand-edits. *Composing the human note in the task body is fine as prose; the
structured state is not.*

## The state machine
```
proposed → active | waiting | abandoned | done
active   → waiting | blocked | done | abandoned
waiting  → active  | blocked | abandoned
blocked  → active  | waiting | abandoned
done, abandoned → (terminal)
```
`done` requires evidence. A same-status update is always allowed (idempotent edit).

## Usage
Needs `fulcra-api` authenticated and `coord-engine` installed (see [Installation](#installation) — any
skill in this family brings the same engine; installing once serves all).
```bash
# create a task doc at team/<team>/task/<slug>.md
coord-engine task start <team> "Fix the widget" \
    --workstream web --priority P1 --status proposed --assignee ash --summary "one-liner"

# move it through the machine (illegal transitions are rejected with a clear error)
coord-engine task update <team> fix-the-widget --status active --next "write the test"
coord-engine task update <team> fix-the-widget --status blocked --blocked-on "waiting on review"

# finish it — evidence is required
coord-engine task done <team> fix-the-widget --evidence "PR #42 merged"
```
Each write stamps `timestamp` and appends a dated note to the task body; the Fulcra File Store versions
every write, so the full history is preserved. After changing tasks, run
`coord-engine reconcile <team>` to refresh the index and views (or let a scheduled reconcile
do it).

See [`references/tasks-cli.md`](references/tasks-cli.md) for the full flag list and the OKF Task shape.
