Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context and Linear sync instructions.

Execute the `/bef sync` command for phase: $ARGUMENTS

If no phase number was provided in $ARGUMENTS, ask the user which phase to sync.

1. First, read `docs/bef/PROJECT_STATE.md`. Verify the target phase has a completed TASKS.md. If not, tell the user to run `/project:bef:deep-dive` first.
2. Read the TASKS.md for the specified phase from `docs/bef/03-phases/phase-NN-[name]/TASKS.md`.
3. Read the phase scope from `docs/bef/02-phase-plan/PHASE_PLAN.md` for the milestone description.
4. Determine the Linear team and project. Ask the user if unclear.
5. Using the Linear MCP tools:
   a. Create a milestone for this phase: "Phase [N]: [Phase Name]"
   b. For each task in TASKS.md, create a Linear issue with:
      - Title from the task
      - Description including scope + acceptance criteria
      - Associated with the milestone
      - Dependency relationships (blocked by / blocking) matching TASKS.md dependencies
      - Labeled with the phase number
6. Record the Linear issue IDs back into TASKS.md in the `Linear ID` field for each task.
7. Update PROJECT_STATE.md: mark the phase's Linear column as complete.
8. Report what was created.
