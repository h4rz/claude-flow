Read `.claude-dev/plan.md` and add a detailed todo list at the end, with all the phases and individual tasks necessary to complete the plan. Each task should be a checkbox item, grouped into **waves** for parallel execution. Tasks within the same wave are independent and can run in parallel. Waves run sequentially.

Format:

## Todo List

### Wave 1 (parallel)
- [ ] Task description → `file/path.ts`
- [ ] Task description → `file/path.ts`

### Wave 2 (parallel, depends on wave 1)
- [ ] Task description → `file/path.ts`
- [ ] Task description → `file/path.ts`

### Wave 3 (sequential)
- [ ] Task description → `file/path.ts`

Tasks should be granular enough that each one can be implemented, verified, and committed independently.

Do not implement yet. Add the todo list only.
