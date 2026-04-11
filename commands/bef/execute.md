Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context and execution rules.

Execute the `/bef execute` command for: $ARGUMENTS

**Determine execution mode from $ARGUMENTS:**
- If $ARGUMENTS contains `--agent-teams` and a phase number (e.g., "3 --agent-teams"): execute phase-level agent teams mode.
- If $ARGUMENTS contains a phase number and task number (e.g., "2 3"): execute single-task mode.
- If $ARGUMENTS is missing required info: ask the user to specify.

## Single-Task Mode (phase + task specified)

1. First, read `docs/bef/PROJECT_STATE.md`. Verify:
   - The target phase has been deep-dived and synced.
   - The target task's dependency tasks within the phase are marked as complete.
   If any gate fails, tell the user what needs to happen first.
2. Read the PHASE_SPEC.md for the full phase context from `docs/bef/03-phases/phase-NN-[name]/`.
3. Read the specific task from TASKS.md (scope, acceptance criteria, dependencies, implementation notes).
4. Read any relevant architecture docs (DATA_MODEL.md for schema work, SYSTEM_ARCHITECTURE.md for API work, etc.).
5. Announce what you're about to build and confirm with the user before proceeding.
6. Build the task. Follow the acceptance criteria exactly.
7. When complete, update:
   - TASKS.md: mark the task status as "Done" with completion date, check off acceptance criteria.
   - PROJECT_STATE.md: update the execution progress count (e.g., "3/8 done").
   - If Linear MCP tools are available, update the Linear issue status.
8. Announce completion and what the next task is.

## Phase-Level Agent Teams Mode (phase + --agent-teams)

1. Read `docs/bef/PROJECT_STATE.md`. Verify the target phase has been deep-dived and synced.
   If gates fail, tell the user what needs to happen first.
2. Read PHASE_SPEC.md, TASKS.md, and relevant architecture docs for the target phase.
3. Follow the "Phase-Level Execution (Agent Teams)" specification in SKILL.md:
   analyze ready tasks, detect file overlaps, form waves, present execution plan,
   dispatch agents in worktrees, process results, cherry-pick, verify, and update state.
4. After all tasks in the phase complete, announce phase completion and suggest `/bef replan`.
