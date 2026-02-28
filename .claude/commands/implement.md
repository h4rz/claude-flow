Read `.claude-dev/plan.md` and implement it all. Follow the plan exactly — it is the spec.

Before starting, quickly identify the project's language, framework, and tooling (check for package.json, Cargo.toml, pyproject.toml, go.mod, Makefile, etc.) so you use the correct type checker, linter, and test runner.

**Create a feature branch before writing any code:**
- Check the current branch with `git branch --show-current`
- Ask me if I want to create a new branch from the current one
- If yes and a Jira ticket is mentioned in the plan or was provided by me (e.g. `PROJ-123`), use it: `git checkout -b feat/PROJ-123-short-description`
- If yes and no ticket, derive a name from the plan's goal: `git checkout -b feat/short-description`
- Branch naming: `feat/`, `fix/`, or `chore/` prefix + kebab-case description

**Parallel execution:** Group tasks into waves based on their dependencies. Independent tasks within the same wave should be executed in parallel using subagents (Task tool). Sequential/dependent tasks run in order. Example:
- Wave 1 (parallel): Create migration file, add service types, write test stubs
- Wave 2 (parallel): Implement service layer, implement API handler (both depend on wave 1)
- Wave 3 (sequential): Integration wiring, final verification (depends on wave 2)

Each subagent should:
- Receive the full task context and relevant file paths from the plan
- Follow all rules below
- Do NOT commit — report back when implementation is done

**After each wave:**
1. Run the project's type checker, linter, or compiler to catch problems early
2. Run verification steps from the plan for that wave's tasks
3. For any tasks involving visual, behavioral, or user-facing changes — **describe what changed and ask for my confirmation** before committing
4. Only after all checks pass and I confirm (if needed): make atomic git commits per task — `type(scope): description`
5. Mark completed tasks in `.claude-dev/plan.md` (change `[ ]` to `[x]`)

Rules:
- Do not stop until all tasks and phases are completed
- Do not add unnecessary comments or docstrings unless the project convention requires them
- Use strict, precise types — avoid loose/dynamic types (e.g. `any` in TS, `Any` in Python, `interface{}` in Go, `Object` in Java)
- Follow existing code patterns, naming conventions, and project structure
- Reuse existing utilities and helpers rather than writing new ones

If implementation reveals a problem not caught during planning, stop and flag it. Describe the specific issue, suggest a fix, and wait for approval before continuing.

Do not add features, optimizations, or "improvements" not in the plan.