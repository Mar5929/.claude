Read the BEF skill file at `skills/bef/SKILL.md` for context.

Execute the `/bef status` command:

1. Read `docs/bef/PROJECT_STATE.md`. If it doesn't exist, tell the user no BEF project is initialized and suggest `/project:bef:init`.
2. Print a clean, formatted summary of:
   - Pipeline progress (which stages are complete)
   - Phase overview table (spec/tasks/linear/execution status per phase)
   - Current focus (what phase and task is active)
   - Any recent replan log entries
3. If there are blocked items or incomplete deep-dives preventing execution, call them out explicitly.
4. Suggest the next logical command the user should run.
