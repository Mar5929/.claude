Read the BEF skill file at `skills/bef/SKILL.md` for the full framework context, interview protocol, and template references.

Execute the `/bef prd` command as defined in the skill file:

1. First, read `docs/bef/PROJECT_STATE.md`. If it doesn't exist, tell the user to run `/project:bef:init` first.
2. Check that this is the correct next stage (Stage 1 should not already be complete, unless resuming).
3. Conduct the PRD interview following the interview protocol in the skill file. Interview topics in order:
   - Business Context
   - User Personas
   - Core Features
   - User Flows
   - Non-Functional Requirements
   - Scope Boundaries
   - Known Constraints
   - Success Criteria
4. Ask ONE focused question at a time. Summarize after every 2-3 exchanges. Probe vague answers. Explicitly transition between topics.
5. When the user signals they're done, read the template at `skills/bef/templates/PRD.md` and produce the final document.
6. Present for review and iterate if needed.
7. When approved, save to `docs/bef/00-prd/PRD.md` and mark Stage 1 complete in PROJECT_STATE.md.
