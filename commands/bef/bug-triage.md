Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context.
Read `skills/bef/modules/bug-management.md` for the bug subsystem specification.

Execute the `/bef:bug-triage` command for: $ARGUMENTS

1. Read `docs/bef/PROJECT_STATE.md`. If it doesn't exist, tell the user to run `/bef:init` first.
2. Determine the bug source from $ARGUMENTS or by asking. Default: `docs/bef/04-bugs/reports/`.
3. Automatically offer to include BEF's PRD and architecture docs as context for the triage agents.
4. Spawn the 3-agent team per the bug-management module spec:
   - Agent 1 (Business Analyst) + Agent 2 (Technical Lead) in parallel
   - Wait for both, then spawn Agent 3 (Triage Coordinator) with their outputs
5. Write the execution plan to `docs/bef/04-bugs/BUG-EXECUTION-PLAN.md` using the template.
6. Update PROJECT_STATE.md Bug Tracking section.
7. Present summary and tell the user to run `/bef:bug-execute [phase]` to start fixing.
