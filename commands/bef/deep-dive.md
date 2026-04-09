Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context, interview protocol, and template references.

Execute the `/bef deep-dive` command for phase: $ARGUMENTS

If no phase number was provided in $ARGUMENTS, ask the user which phase they want to deep dive.

1. First, read `docs/bef/PROJECT_STATE.md`. Verify:
   - Stage 3 (Phase Plan) is complete.
   - All dependency phases for the target phase already have their deep-dives complete.
   If either gate fails, tell the user what needs to happen first.
2. Read: `docs/bef/02-phase-plan/PHASE_PLAN.md`, all architecture docs from `docs/bef/01-architecture/`, and any completed PHASE_SPEC.md files for dependency phases.
3. Announce the deep dive and begin the interview across five areas (one at a time):
   a. **Functional Requirements** — Get specific about what this phase builds.
   b. **Technical Approach** — Propose an approach, discuss alternatives.
   c. **Edge Cases & Error Handling** — List edge cases you see, ask what you're missing.
   d. **Integration Points** — Confirm contracts with prior/future phases.
   e. **Acceptance Criteria** — Propose what "done" looks like, refine together.
4. Follow the interview protocol: one question at a time, summarize periodically, probe vague answers, ask for closure before moving on.
5. When the interview is complete, read the templates from `skills/bef/templates/PHASE_SPEC.md` and `skills/bef/templates/TASKS.md`.
6. Produce both PHASE_SPEC.md and TASKS.md for this phase.
7. Present both for review and iterate.
8. When approved, create the phase directory `docs/bef/03-phases/phase-NN-[name]/` and save both files.
9. Update PROJECT_STATE.md: mark the phase's Spec and Tasks columns as complete.
