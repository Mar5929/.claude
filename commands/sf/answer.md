---
name: sf:answer
description: Record an answer to an open question
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
---
<objective>
Mark an open question as answered, record the answer, and assess downstream impact
on designs, blocked work, and related questions.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
Execute the "Question Answered" workflow from SKILL.md.

1. Move the question from Open to Answered in the appropriate questions.md.
2. Record the answer text and date.
3. Impact assessment (do ALL):
   a. Check if the answer changes any existing technical design. If yes, update the design doc.
   b. Check if the answer unblocks any work items. If yes, note them as unblocked.
   c. Check if the answer raises new questions. If yes, create them.
   d. Check if the answer contradicts any existing decision. If yes, flag the conflict.
4. Update the Impact field on the question with links to everything changed.
5. Report back: "I've marked Q-XX-YYY as answered. Here's what changed: ..."

If the user provided a question ID and answer as arguments, use them directly. Otherwise, ask which question to answer.
</process>
