---
name: sf:wrap-up
description: End-of-session summary and log
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
---
<objective>
Generate an end-of-session summary covering all changes made during this session,
and append it to the session log.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
Execute the "Session Wrap-Up" workflow from SKILL.md.

1. Use git diff on framework files to identify all changes made during this session.
2. Read all questions.md files for questions raised or answered this session.
3. Read engagement/decisions.md and epic-level decisions.md for decisions made.
4. Read engagement/execution-plan.md for execution changes.
5. Check engagement/meeting-notes/ for any notes created this session.
6. Generate a session summary and append to engagement/session-log.md:
   - What Happened: questions raised/answered, decisions, designs updated, stories created
   - Files Changed: list with what changed in each
   - Loose Ends: questions raised but not owned, decisions not reflected in designs, action items not converted to tasks
   - Pick Up Next: specific next steps, prioritized
7. Report the summary to the user.
</process>
