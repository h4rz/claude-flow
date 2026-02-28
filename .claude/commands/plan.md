I want to: $ARGUMENTS

First, ensure the `.claude-dev/` directory exists and is gitignored:
- Create `.claude-dev/` if it doesn't exist
- Add `.claude-dev/` to `.gitignore` if not already present

Spawn the planning as an Opus subagent for best results. Write the planning prompt to `.claude-dev/prompt.md` with these instructions:

```
I want to: [insert $ARGUMENTS]

Read the relevant source files first. If .claude-dev/research.md exists, read it for context. Base the plan entirely on the actual codebase — do not guess at file contents or project conventions.

Write a detailed .claude-dev/plan.md document (overwrite any existing file) outlining how to implement this. The plan must include:

- A clear goal statement (business outcome, not just technical change)
- Detailed explanation of the approach and why this approach over alternatives
- Concrete code snippets showing the actual changes
- File paths for every file that will be modified or created
- Considerations, trade-offs, and potential risks
- Verification steps — how to confirm each part works after implementation

For each task, mark whether it depends on another task or can run independently — this enables parallel execution later.

Do not implement. Write the plan only.
```

Then run:
```bash
env -u CLAUDECODE claude -p --model claude-opus-4-6 --max-turns 25 --allowedTools "Read,Write,Glob,Grep,Bash(find:*),Bash(grep:*),Bash(cat:*),Bash(ls:*),Bash(head:*),Bash(tail:*),Bash(wc:*)" < .claude-dev/prompt.md
```

If that fails (Opus not available, CLI error, etc.), fall back to doing the planning directly in the current session using the same instructions.

After the plan is written, read `.claude-dev/plan.md` and present a summary. If there are open questions about scope, constraints, or trade-offs that would significantly change the approach, ask me now.

Do not implement yet.
