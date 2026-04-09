Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context.

Execute the `/bef amend` command for phase: $ARGUMENTS

If no phase number was provided in $ARGUMENTS, ask the user which phase to amend.

1. Read the current PHASE_SPEC.md and TASKS.md for the target phase from `docs/bef/03-phases/phase-NN-[name]/`.
2. Interview the user:
   - "What's changing? New requirements, removed requirements, or modified approach?"
   - Probe for specifics on each change.
3. Update PHASE_SPEC.md with the changes. Add an entry to the Revision History section at the bottom documenting what changed and why.
4. Update TASKS.md: add new tasks, remove obsolete ones, adjust existing ones as needed.
5. If the phase has already been synced to Linear (check PROJECT_STATE.md), inform the user they should re-run `/bef:sync [phase#]` to update Linear.
6. Update PROJECT_STATE.md to reflect any changes.
