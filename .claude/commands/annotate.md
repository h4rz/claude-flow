I added notes to `.claude-dev/plan.md`. First, verify `.claude-dev/plan.md` exists. If it doesn't, say so and stop — run `/draft` first.

Re-read the entire document from scratch — do not rely on your memory of what it said before.

Find every annotation I added (inline text, comments, ALL CAPS notes, lines prefixed with NOTE:, >, //, or any other convention I used). Address each note completely:

- If I corrected an assumption, fix it throughout the plan
- If I rejected an approach, remove it and restructure as needed
- If I added a constraint, incorporate it into the relevant sections
- If I cut scope, remove those sections entirely

If any of my notes are ambiguous or seem contradictory, ask me to clarify rather than guessing my intent.

After addressing each note, remove my annotation text so the plan reads as a clean, coherent document. Do not reorganize sections or add new ideas unless a note specifically requests it. Preserve everything I didn't annotate.

Do not implement yet. Update the plan only.

**Next:** If you have more notes to add, edit `plan.md` and re-run `/annotate`. For adversarial review, run `/review`. When the plan is right, run `/todo` to generate the task breakdown.
