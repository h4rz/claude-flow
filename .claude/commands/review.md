Review the current plan for problems, gaps, and better alternatives.

First, verify `.claude-dev/plan.md` exists. If it doesn't, say so and stop — there's nothing to review.

Spawn the review as an Opus subagent for best results. Write the review prompt to `.claude-dev/prompt.md` with these instructions:

```
You are a senior engineer reviewing a plan before implementation begins. Your job is to find problems — assume the plan author is competent and doesn't need encouragement.

Read `.claude-dev/plan.md` carefully. Also read `.claude-dev/research.md` and `.claude-dev/interview.md` if they exist — these provide context on what was already explored and what constraints were agreed upon.

Then read the actual source files referenced in the plan. Verify that the plan's assumptions about the codebase are correct.

Write `.claude-dev/review.md` with your findings. Organize into these sections (skip any section with no findings):

## Critical Issues
Problems that would cause the implementation to fail, break existing functionality, or violate stated constraints. For each:
- What the problem is
- Where in the plan it occurs
- Why it's a problem (concrete scenario, not theoretical)
- Suggested fix

## Missing Edge Cases
Scenarios the plan doesn't handle that it should. For each:
- The scenario
- What would happen with the current plan
- What should happen instead

## Incorrect Assumptions
Places where the plan assumes something about the codebase that isn't true. For each:
- The assumption (quote the plan)
- What the code actually does (with file path and evidence)
- How the plan should be adjusted

## Questionable Decisions
Choices that aren't wrong but have better alternatives. For each:
- The current approach
- The alternative
- Trade-offs (be specific — not just "more maintainable")

## Missing from Plan
Things the plan should address but doesn't: missing verification steps, unaddressed task dependencies, files that need changes but aren't listed, rollback or failure handling.

End with a one-paragraph summary: is this plan ready for implementation, or does it need revisions first?

Be specific — reference file paths, function names, and line numbers. Do not make vague criticisms. Do not mention what's good — focus entirely on what needs to change.

Do not modify plan.md. Write review.md only.
```

Then run:
```bash
env -u CLAUDECODE claude -p --model claude-opus-4-6 --max-turns 25 --allowedTools "Read,Write,Glob,Grep,Bash(ls:*),Bash(wc:*)" < .claude-dev/prompt.md
```

If that fails (Opus not available, CLI error, etc.), fall back to doing the review directly in the current session using the same instructions.

After the review is written, read `.claude-dev/review.md` and present the findings organized by severity:

1. **Critical Issues** (must fix before implementing)
2. **Incorrect Assumptions** (likely must fix)
3. **Missing Edge Cases** (should fix)
4. **Questionable Decisions** (discuss)
5. **Missing from Plan** (consider)

For each finding, ask: **integrate, skip, or discuss further?**

After I decide on each item, collect the ones marked "integrate" and add them as inline `NOTE:` annotations in `.claude-dev/plan.md` above the relevant section (the format `/annotate` already recognizes). Then tell me to run `/annotate` to process them.

If no findings were marked "integrate," confirm the plan is review-approved and ready for `/todo`.
