Verify the work done in `.claude-dev/plan.md`. First, verify `.claude-dev/plan.md` exists. If it doesn't, say so and stop — run `/draft` first.

Before starting, identify the project's test framework and existing test patterns (check for jest.config, vitest.config, pytest.ini, *_test.go, etc.).

Read the plan and for each requirement it was supposed to satisfy:

1. **Generate scoped tests** — Read the completed tasks (`[x]`) and the modified source files. Identify the blast radius — components that depend on or are affected by the changes. Then:
   - Write unit tests for new or modified functions (happy path, edge cases, error handling)
   - Write regression tests for existing functionality that touches modified code paths
   - Follow the project's existing test conventions (file location, naming, assertion style, mocking)
   - Do NOT test unchanged, unrelated code — stay within the scope of the plan
   - If existing tests already cover a scenario, skip it — do not duplicate coverage
   - Commit: `test(scope): description`

2. **Run automated checks** — Run the full test suite (including the new tests), type checker, linter, and any verification steps listed in the plan. Report results.

3. **Walk me through manual checks** — For each deliverable, tell me exactly what to test and what to expect. Wait for my confirmation (pass/fail) on each one.

4. **For any failures** — Investigate the root cause, describe what's wrong, suggest a specific fix, and wait for my approval before fixing.

Keep it focused — generate the right tests, then confirm the plan was delivered correctly.

**Next:** For failures that need dedicated investigation, use `/fix <issue>`. When all checks pass, the task is complete — run `/cleanup` to remove working files, or `/research <area>` to start a new task.
