Read the BEF skill file at `skills/bef/SKILL.md` for context.
Read `skills/bef/modules/bug-management.md` for the bug tracking summary spec.

Execute the `/bef:status` command:

1. Read `docs/bef/PROJECT_STATE.md`. If it doesn't exist, tell the user no BEF project is initialized and suggest `/bef:init`.
2. Print a clean, formatted summary of:
   - Pipeline progress (which stages are complete)
   - Phase overview table (spec/tasks/linear/execution status per phase)
   - Current focus (what phase and task is active)
   - Any recent replan log entries
3. If there are blocked items or incomplete deep-dives preventing execution, call them out explicitly.
4. If `docs/bef/04-bugs/BUGS.md` exists, include the Bug Tracking summary (open/fixed counts by severity, active bug phase).
5. Suggest the next logical command the user should run.
