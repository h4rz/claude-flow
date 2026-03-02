I want to: $ARGUMENTS

First, ensure the `.claude-dev/` directory exists and is gitignored:
- Create `.claude-dev/` if it doesn't exist
- Add `.claude-dev/` to `.gitignore` if not already present

**Structured interview — scaled to task complexity:**

Read `.claude-dev/research.md` if it exists. Based on the research (if any) and the task description, assess the task complexity:

- **Small** (single file change, config tweak, bug fix with known cause): Skip the interview. Note "Interview skipped — straightforward task" and proceed to planning.
- **Medium** (feature touching 2–5 files, clear scope): Ask 3–5 questions from the most relevant categories below.
- **Large** (cross-cutting feature, new subsystem, migration, multi-service change): Ask 6–10 questions across multiple categories.

Question categories — pick only the ones relevant to this specific task:

**Architecture & Approach:**
- Are there existing patterns in the codebase I should follow, or is this greenfield?
- Any preference between [specific trade-off relevant to this task — e.g. sync vs async, SQL vs NoSQL]?
- Should this be implemented as [option A] or [option B]? (only if research surfaced multiple viable approaches)

**Scope & Boundaries:**
- What's explicitly out of scope for this change?
- Should this handle [edge case] now, or is that a follow-up?
- Any backward compatibility requirements?

**Data & State:**
- What's the data model — new tables/collections, or extending existing ones?
- Any migration strategy preferences (zero-downtime, maintenance window)?
- Caching or performance requirements?

**Testing & Quality:**
- What level of test coverage is expected (unit, integration, e2e)?
- Are there specific scenarios that must be tested?
- Any existing test patterns I should follow?

**Deployment & Operations:**
- Any feature flag or rollout strategy needed?
- Environment-specific configuration concerns?
- Monitoring or alerting requirements?

**Do not ask questions whose answers are already clear from the research or the task description.** If `research.md` already states the project uses a specific pattern or technology, incorporate that as a known constraint rather than asking about it.

Present questions grouped by category. Wait for my answers. Then write `.claude-dev/interview.md`:

```
## Interview — [brief task description]

**Task complexity:** [Small/Medium/Large]
**Date:** [current date]

### [Category Name]
**Q:** [question]
**A:** [my answer]

...

### Constraints Derived
- [constraint 1 — synthesized from the answers above]
- [constraint 2]
- ...
```

If the task was Small, write: `## Interview skipped — straightforward task` and nothing else.

Spawn the planning as an Opus subagent for best results. Write the planning prompt to `.claude-dev/prompt.md` with these instructions (include the derived constraints at the top):

```
I want to: [insert $ARGUMENTS]

## Constraints (from interview)
[list every constraint derived from the interview — these are non-negotiable requirements. Omit this section if the interview was skipped.]

Read the relevant source files first. If .claude-dev/research.md exists, read it for context. If .claude-dev/interview.md exists, read it for the full Q&A context. Base the plan entirely on the actual codebase — do not guess at file contents or project conventions.

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
env -u CLAUDECODE claude -p --model claude-opus-4-6 --max-turns 25 --allowedTools "Read,Write,Glob,Grep,Bash(ls:*),Bash(wc:*)" < .claude-dev/prompt.md
```

If that fails (Opus not available, CLI error, etc.), fall back to doing the planning directly in the current session using the same instructions.

After the plan is written, read `.claude-dev/plan.md` and present a summary. Ask about any remaining open questions not already resolved above.

Do not implement yet.

**Next:** Review `plan.md`. Add inline notes and run `/annotate` to process them (repeat until right). Run `/review` for adversarial critique. When the plan is ready, run `/todo` to generate the task breakdown.
