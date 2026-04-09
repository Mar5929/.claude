Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context and replan rules.

Execute the `/bef replan` command:

1. Read: `docs/bef/PROJECT_STATE.md`, `docs/bef/02-phase-plan/PHASE_PLAN.md`, `docs/bef/00-prd/PRD.md`, and all completed PHASE_SPEC.md files from `docs/bef/03-phases/`.
2. Assess:
   - Did the most recently completed phase deliver everything in its spec?
   - Are there gaps between what's been built so far and what the PRD requires?
   - Do any future phase specs or the phase plan need adjustment based on what was learned during implementation?
3. Report findings to the user. Propose specific changes if needed.
4. If changes are needed:
   - Update `docs/bef/02-phase-plan/PHASE_PLAN.md` with revised phases, ordering, or scope.
   - If a new phase is needed, add it with the next sequential phase number.
   - If tasks need to be added to an existing future phase, note them for the next deep-dive.
5. Append a dated entry to the Replan Log in PROJECT_STATE.md summarizing what was found and what changed.

**If all phases are complete:** Run a full PRD audit. Compare everything built across all phases against every requirement in the PRD. Flag gaps, drift, and missing functionality. Propose remediation phases if needed.
