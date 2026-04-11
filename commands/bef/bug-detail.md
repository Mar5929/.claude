Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context.
Read `skills/bef/modules/bug-management.md` for the bug subsystem specification.

Execute the `/bef:bug-detail` command for: $ARGUMENTS

1. Read `docs/bef/PROJECT_STATE.md`. If it doesn't exist, tell the user to run `/bef:init` first.
2. Check the Bug Detailer Config in PROJECT_STATE.md. If not configured, run the setup interview
   per the bug-management module spec. If CLAUDE.md has a `## Bug Detailer Config` section,
   offer to migrate it.
3. Get the bug description from the user (from $ARGUMENTS or by asking).
4. Investigate with Playwright following the full investigation workflow in the module spec:
   start dev server → navigate → authenticate → reproduce → gather evidence → analyze → classify.
5. Write the bug report to `docs/bef/04-bugs/reports/BUG-XXX.md` using the template.
6. Update `docs/bef/04-bugs/BUGS.md` and PROJECT_STATE.md Bug Tracking section.
7. Present the summary and ask if the user wants to investigate another bug.
