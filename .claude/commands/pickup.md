Resume a previously paused or interrupted session.

1. **Find the best recovery source.** Check for both files:
   - `.claude-dev/session.md` — manually saved by `/park` (richer context: blockers, next steps, uncommitted change notes)
   - `.claude-dev/progress.md` — auto-saved by `/implement` after each completed wave (lightweight checkpoint)

   If neither exists, say so and stop — there's nothing to resume.

   If both exist, compare their timestamps. Use the **newer one** for position data (which wave, which task). Also read the older one for any blockers or decisions it recorded.

2. Read `.claude-dev/plan.md` for the full plan, task list, and progress markers.

3. Verify the current state matches what was saved:
   - Run `git branch --show-current` and check it matches the saved branch
   - Run `git status` to see if there are uncommitted changes carried over
   - Run `git log --oneline -10` and confirm recent commits match what was saved

4. If there are uncommitted changes from the previous session, describe each one and ask me how to handle them (commit, discard, or continue working on them) before proceeding.

5. Present a brief status summary:
   - **Recovery source:** which file(s) were used
   - What's been completed (waves/tasks with commits)
   - What's next (current wave, next task)
   - Any blockers or pending decisions from the session state

6. Ask me to confirm, then continue implementation from where it left off — following the same process as `/implement`:
   - Run remaining tasks in parallel waves
   - Run type checker/linter after each wave
   - For visual or behavioral changes, describe what changed and ask for my confirmation
   - Atomic git commit per task: `type(scope): description`
   - Mark completed tasks in `.claude-dev/plan.md` (`[ ]` → `[x]`)
   - **Continue auto-saving** `.claude-dev/progress.md` after each completed wave

Do not re-do completed tasks. Start from the first incomplete task in the current wave.
