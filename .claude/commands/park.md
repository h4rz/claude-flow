Pause the current session and save state for resuming later.

1. Read `.claude-dev/plan.md` and identify:
   - Which tasks are completed (`[x]`) vs remaining (`[ ]`)
   - Which wave you're currently in

2. Capture the current codebase state:
   - Run `git branch --show-current` to get the active branch
   - Run `git status` to see uncommitted changes
   - Run `git log --oneline -10` to see recent commits

3. Write `.claude-dev/session.md` with this structure:

```
## Session State

**Paused at:** [date and time]
**Current branch:** [branch name]

### Progress
- Completed waves: [list wave numbers that are fully done]
- Current wave: [wave number in progress]
- Next task: [exact task description from the todo list]

### Recent Commits
[last 5-10 commits from git log]

### Uncommitted Changes
[git status output — for each changed file, note what the change is for]

### Blockers or Pending Decisions
[anything unresolved that the next session needs to know, or "None"]

### Next Steps
[step-by-step: exactly what to do when resuming, specific enough that a fresh session can continue without re-reading all the code]
```

4. If there are uncommitted changes that are ready to commit (type checker/linter passes), ask me if I want to commit them before pausing. If they're not ready, note them explicitly in session.md so the next session knows to handle them.

5. Confirm the session state has been saved and that `/pickup` will be able to pick up from here.
