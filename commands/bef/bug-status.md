Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context.
Read `skills/bef/modules/bug-management.md` for the bug subsystem specification.

Execute the `/bef:bug-status` command.

1. Read `docs/bef/PROJECT_STATE.md`. If it doesn't exist, tell the user to run `/bef:init` first.
2. Read `docs/bef/04-bugs/BUGS.md` if it exists.
3. Print the bug tracking summary per the bug-management module spec:
   open/fixed counts by severity, active bug phase progress, bugs blocking BEF phases.
