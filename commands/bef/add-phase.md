Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context.

Execute the `/bef add-phase` command:

1. Read: `docs/bef/PROJECT_STATE.md`, `docs/bef/02-phase-plan/PHASE_PLAN.md`, and all completed PHASE_SPEC.md files.
2. Interview the user:
   - "What does this new phase need to accomplish?"
   - "What existing phases does it depend on?"
   - "Does it block any future phases?"
3. Assign the next sequential phase number (even if it logically fits between earlier phases — ordering is handled by the dependency graph, not numbering).
4. Add the phase to `docs/bef/02-phase-plan/PHASE_PLAN.md` with full details: scope, dependencies, unlocks, complexity estimate.
5. Update the Phase Overview table in PROJECT_STATE.md with the new phase row.
6. Inform the user to run `/bef:deep-dive [N]` when ready to spec it out.
