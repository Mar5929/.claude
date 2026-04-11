Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context.
Read `skills/bef/modules/bug-management.md` for the bug subsystem specification.
Read `skills/bef/modules/parallel-orchestration.md` for the shared agent dispatch pattern.

Execute the `/bef:bug-execute` command for: $ARGUMENTS

1. Read `docs/bef/PROJECT_STATE.md`. If it doesn't exist, tell the user to run `/bef:init` first.
2. Read `docs/bef/04-bugs/BUG-EXECUTION-PLAN.md`. If it doesn't exist, tell the user to
   run `/bef:bug-triage` first.
3. Parse phases and select one based on $ARGUMENTS (phase number, "next", or show status and ask).
4. Spawn developer agents per the bug-management module spec, using the parallel orchestration
   module for dispatch, result processing, cherry-pick, verify, and cleanup.
5. After merge, update: bug detail files (status → Fixed), BUGS.md, execution plan
   (completion markers), and PROJECT_STATE.md Bug Tracking section.
6. Announce completion and what the next phase is.
