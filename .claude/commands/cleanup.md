Clean up the `.claude-dev/` working directory.

**Warning:** If an implementation is in progress, this will destroy `session.md` and `progress.md` recovery data. Only run this after `/verify` confirms the task is complete, or to abandon a task entirely.

Delete all files inside `.claude-dev/` (research.md, interview.md, plan.md, review.md, prompt.md, progress.md, session.md, and any other files created during the workflow). Keep the directory itself and the `.gitignore` entry.

Confirm what was deleted.

Note: `/research` automatically cleans up on start — this command is for manual cleanup between steps or when you want to reset without starting a new task.

**Next:** To start a new task, run `/research <area>`.
