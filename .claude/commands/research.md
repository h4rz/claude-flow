Research this area of the codebase in depth: $ARGUMENTS

First, set up a clean workspace:
- Create `.claude-dev/` if it doesn't exist
- Add `.claude-dev/` to `.gitignore` if not already present
- Delete all existing files inside `.claude-dev/` (clean slate for new task)

Spawn the research as an Opus subagent for best results. Write the research prompt to `.claude-dev/prompt.md` with these instructions:

```
Read the following area of the codebase in depth: [insert $ARGUMENTS]

Understand how it works deeply — follow call chains, read implementation bodies, understand data flows end-to-end. Do not skim or read only function signatures. Pay attention to:

- How data flows through the system (inputs → transformations → outputs)
- Error handling patterns and edge cases already accounted for
- Caching layers, middleware, hooks, and interceptors
- Naming conventions and code style patterns
- Database schema, ORM conventions, and migration patterns
- Existing utility functions and shared modules that could be reused
- Test patterns and what's already covered

Write a detailed report of your learnings and findings in .claude-dev/research.md (overwrite any existing file). Include file paths and code references throughout. Flag any uncertainties explicitly.

Do not plan or implement anything. Research only.
```

Then run:
```bash
env -u CLAUDECODE claude -p --model claude-opus-4-6 --max-turns 25 --allowedTools "Read,Write,Glob,Grep,Bash(ls:*),Bash(wc:*)" < .claude-dev/prompt.md
```

If that fails (Opus not available, CLI error, etc.), fall back to doing the research directly in the current session using the same instructions.

After research completes, read `.claude-dev/research.md` and present a brief summary. If anything is ambiguous or there are conflicting patterns, ask me before proceeding.

Do not plan or implement anything.
