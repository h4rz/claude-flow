# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A collection of Claude Code slash commands for plan-driven development. Commands live in `.claude/commands/` as `.md` files and are installed into a target project's `.claude/commands/` directory (or `~/.claude/commands/` for global use).

No build system, tests, or package manager — this is a documentation/prompt repository.

## Commands

Each file in `.claude/commands/` corresponds to a slash command:

| File | Slash Command | Role |
|------|--------------|------|
| `research.md` | `/research` | Deep codebase read → `.claude-dev/research.md` (spawns Opus) |
| `draft.md` | `/draft` | Structured interview + write `.claude-dev/plan.md` (spawns Opus) |
| `review.md` | `/review` | Adversarial plan review → `.claude-dev/review.md` (spawns Opus) |
| `annotate.md` | `/annotate` | Process inline notes in plan, clean document |
| `todo.md` | `/todo` | Append wave-based task checklist to plan |
| `implement.md` | `/implement` | Execute plan with parallel waves + atomic commits + auto-save |
| `park.md` | `/park` | Save session state to `.claude-dev/session.md` for resuming later |
| `pickup.md` | `/pickup` | Restore session state (from `session.md` or `progress.md`) and continue |
| `verify.md` | `/verify` | Generate scoped tests + run checks + walk through manual verification |
| `fix.md` | `/fix` | Investigate, fix, verify, commit a specific issue |
| `cleanup.md` | `/cleanup` | Wipe `.claude-dev/` working files |

## Workflow Architecture

**Working directory:** `.claude-dev/` (auto-created, auto-gitignored). Contains `research.md`, `interview.md`, `plan.md`, `review.md`, `prompt.md`, `progress.md`, `session.md`. Entry-point command (`/research`) wipes it on start.

**Opus subagent pattern:** `/research`, `/draft`, and `/review` write a prompt to `.claude-dev/prompt.md` then spawn Opus via:
```bash
env -u CLAUDECODE claude -p --model claude-opus-4-6 --max-turns 25 --allowedTools "Read,Write,Glob,Grep,Bash(ls:*),Bash(wc:*)" < .claude-dev/prompt.md
```
Falls back to current session model if Opus is unavailable.

**Parallel execution:** `/todo` groups tasks into waves (independent tasks within a wave run in parallel via subagents; waves run sequentially). `/implement` executes this wave structure.

**Commit discipline:** Atomic commits per task — `type(scope): description`. Only after type checker/linter passes and user confirms visual/behavioral changes. Branch naming: `feat/`, `fix/`, or `chore/` prefix + kebab-case. With a Jira ticket: `feat/PROJ-123-short-description`.

**Pipeline order:**
```
/research → /draft (with interview) → /review (optional) → /annotate (repeat) → /todo → /implement (with auto-save) → /verify → /fix (repeat)
```

**Session persistence:** `/park` saves current wave progress, git state, uncommitted changes, and next steps to `.claude-dev/session.md`. `/implement` auto-saves a lightweight checkpoint to `.claude-dev/progress.md` after each completed wave. `/pickup` recovers from whichever is newer.

**Command interactions:**
- `/draft` does NOT wipe `.claude-dev/` — it preserves existing `research.md` to reference during planning
- `/draft` writes `.claude-dev/interview.md` when the task warrants questions (skips for small tasks)
- `/research` wipes `.claude-dev/` (clean slate)
- `/review` does NOT modify `plan.md` — it writes `review.md` and adds `NOTE:` annotations for `/annotate` to process
- `/annotate` can be run repeatedly until the plan is approved
- `/implement` marks tasks complete by changing `[ ]` to `[x]` in `plan.md` and auto-saves `progress.md` after each wave
- `/pickup` checks both `session.md` and `progress.md`, using the newer one for position data

**Escalation gate:**
- `/fix` — if the fix requires design changes, stop and describe; user may want to update the plan first

## Making Changes

When editing commands, preserve:
- The Opus spawn pattern (fallback behavior matters)
- The `.claude-dev/` directory convention
- User confirmation checkpoints between major steps
- The atomic commit + verification gate before committing
- Wave-based parallel execution structure in `implement.md` and `todo.md`
- Escalation gate in `/fix`
- Coding rules enforced by `/implement`: no extra comments/docstrings, strict types (no `any`/`Any`/`interface{}`/`Object`), follow existing patterns, no unplanned features, stop-and-flag on discovered problems
