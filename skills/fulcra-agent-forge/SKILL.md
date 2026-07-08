---
name: fulcra-agent-forge
description: "Bridge GitHub into a fulcra-agent-teams review flow: mirror PR state onto the team store as evidence, and auto-approve a review when its PR merges — so review status reflects the forge even when no human files a verdict."
homepage: "https://github.com/fulcradynamics/agent-skills"
license: "MIT"
user-invocable: true
metadata: { "openclaw": { "emoji": "🔗" } }
---

# Fulcra Agent Forge

## Installation

This skill uses the `coord-engine` CLI — a small, stdlib-only tool that runs the deterministic folds. Install it once:

```bash
uv tool install "git+https://github.com/ashfulcra/fulcra-tools@coord-engine-v1.3.0#subdirectory=packages/coord-engine"
```

After installation, `coord-engine <command>` is on your PATH.

Enhances the `fulcra-agent-review` skill (proposed in PR #129). When a review's `artifact:` is a
GitHub PR URL, the forge is the ground truth — this skill mirrors it onto the team store so
`coord-engine review status` reflects reality:

- **Evidence**: one idempotent shard per PR state transition
  (`_coord/evidence/<slug>/state-<OPEN|MERGED|CLOSED>.md`).
- **Auto-verdict**: when the PR merges, a `verdicts/forge.md` approval is written (reviewer `forge`) —
  the review tally then folds it like any reviewer.

## Usage
```bash
coord-engine forge mirror <team>   # one pass over all PR-backed reviews
```
Run it ad hoc, or on the heartbeat alongside `reconcile`. **Requires the GitHub CLI (`gh`) authenticated**;
without it the command is a clear no-op (exit 0) — the skill degrades, nothing breaks.

## Notes
- Only `github.com/<owner>/<repo>/pull/<n>` artifacts are mirrored; other artifacts are skipped silently.
- A failing `gh` call leaves the review untouched (never fabricates state).
- The forge approval doesn't override human verdicts — a human `changes` still blocks (CHANGES dominates).
