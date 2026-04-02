---
name: sf:ask
description: Raise a new question in the knowledge base
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
---
<objective>
Create a new question entry in the appropriate questions.md file, with scope, owner,
and blocking information.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
Execute the "Question Raised" workflow from SKILL.md.

1. Read references/question-system.md for the full question lifecycle and ID scheme.
2. Determine scope: engagement-wide, epic, or feature.
3. Assign an ID: Q-{SCOPE}-{NUMBER} (e.g., Q-ENG-001, Q-DM-003).
4. Add the question to the appropriate questions.md under the Open section.
5. Set owner (ask if unclear).
6. Identify what work this question blocks and link to design sections, user stories, or tasks.
7. Confirm back what was created and where.

If the user provided a question as an argument, use it directly. Otherwise, ask what question to raise.
</process>
