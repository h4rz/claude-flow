Fix the following issue: $ARGUMENTS

First, verify `.claude-dev/plan.md` exists. If it doesn't, say so and stop — run `/draft` first.

Read `.claude-dev/plan.md` to understand the original intent and what was built.

1. **Investigate** — Read the relevant code, reproduce the issue, and identify the root cause. Do not guess — trace the actual code path.

2. **Explain** — Describe what's wrong and why, referencing specific files and lines.

3. **Fix** — Make the minimal, targeted correction. Do not refactor, reorganize, or "improve" anything beyond what's needed to fix this specific issue.

4. **Verify** — Run the project's type checker/linter, relevant tests, and any related verification steps from the plan. For visual or behavioral changes, **show me what changed and ask for my confirmation** before proceeding.

5. **Commit** — Only after verification passes (including my confirmation if needed): atomic git commit `fix(scope): description`

If the fix is more complex than expected (requires design changes or touches the plan's architecture), stop and describe what's needed — I may want to update the plan first rather than patch around it.

**Next:** Run `/verify` to confirm the fix works and nothing else broke.
