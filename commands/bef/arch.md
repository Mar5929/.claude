Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context and template references.

Execute the `/bef arch` command as defined in the skill file:

1. First, read `docs/bef/PROJECT_STATE.md`. Verify Stage 1 (PRD) is complete. If not, tell the user to complete it first.
2. Read `docs/bef/00-prd/PRD.md`.
3. Read the four architecture templates from `skills/bef/templates/`:
   - TECH_STACK.md
   - DATA_MODEL.md
   - SYSTEM_ARCHITECTURE.md
   - INTEGRATION_MAP.md
4. Generate drafts of all four architecture documents based on the PRD.
5. Present each document ONE at a time for the user's review.
6. For each, ask: "Does this look right? Anything to change or add?"
7. Iterate until the user approves each one.
8. Save all four to `docs/bef/01-architecture/`.
9. Mark Stage 2 complete in PROJECT_STATE.md.
