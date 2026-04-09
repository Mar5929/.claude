Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context and template references.

Execute the `/bef adopt` command as defined in the skill file:

1. Create the `docs/bef/` directory structure.
2. Interview the user about the current state of the project:
   - What stage are they at? What's been built?
   - Do existing planning documents exist? Where are they?
   - How many phases, and which are done?
3. Migrate existing docs into the standard BEF artifact format using the templates in `skills/bef/templates/`.
4. For completed phases, mark them done and create retroactive summaries. Do NOT require re-speccing completed work.
5. For the current in-progress phase, create the spec and tasks, ask which tasks are done.
6. Update PROJECT_STATE.md to reflect the true current state.
