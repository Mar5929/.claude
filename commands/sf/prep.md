---
name: sf:prep
description: Generate a meeting preparation brief
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
---
<objective>
Generate a meeting preparation brief with suggested agenda, prioritized questions,
decisions needed, and items requiring client input.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
Execute the "Meeting Preparation" workflow from SKILL.md.

1. Determine meeting type: discovery, design review, status, or ad hoc.
2. Read all questions.md files for open questions, prioritized by blocking impact.
3. Read all decisions.md for pending or recently made decisions.
4. Read engagement/execution-plan.md for blocked items and upcoming work.
5. Read engagement/roadmap.md for approaching milestones.
6. Read engagement/risks.md for active risks.
7. Read recent engagement/meeting-notes/*.md for continuity.
8. Generate a meeting preparation brief:
   - Suggested agenda with time allocations
   - Prioritized questions to raise (grouped by epic)
   - Decisions needed from the client
   - Items requiring client input
   - Demo or review items ready for sign-off
   - Risks to discuss
9. Optionally create a stub meeting note with the generated agenda.
</process>
