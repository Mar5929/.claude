Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context and template references.

Execute the `/bef phases` command as defined in the skill file:

1. First, read `docs/bef/PROJECT_STATE.md`. Verify Stage 2 (Architecture) is complete. If not, tell the user to complete it first.
2. Read `docs/bef/00-prd/PRD.md` and all four architecture docs from `docs/bef/01-architecture/`.
3. Read the phase plan template from `skills/bef/templates/PHASE_PLAN.md`.
4. Propose the full Phase Plan with:
   - Phase numbers and names
   - One-paragraph scope per phase
   - Dependencies (what must come first)
   - What each phase unlocks
   - Complexity estimates (S/M/L/XL)
   - Parallelism opportunities
5. Follow the decomposition guidelines in the skill file (schema first, auth early, core logic before UI, etc.).
6. Present for review. Discuss ordering, merging/splitting phases.
7. When approved, save to `docs/bef/02-phase-plan/PHASE_PLAN.md`.
8. Update PROJECT_STATE.md: mark Stage 3 complete and populate the Phase Overview table.
