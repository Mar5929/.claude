---
name: sf:process
description: Process a transcript or brain dump into framework files
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
---
<objective>
Extract questions, answers, decisions, requirements, and action items from unstructured
input (transcript, brain dump, notes) and file them into the appropriate framework files.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
Execute the "Transcript or Brain Dump Processing" workflow from SKILL.md.

1. Save the raw input to references/{date}-{description}.md.
2. Extract and categorize:
   - Questions raised: create in appropriate questions.md
   - Questions answered: find matching open questions, mark answered, assess impact
   - Decisions made: record in appropriate decisions.md
   - Requirements stated: add to engagement/requirements.md
   - Action items: note them and ask user if they should become tasks
   - New requirements or scope changes: flag for user confirmation before filing
3. If the input is a structured meeting (clear date, attendees, agenda), also create a meeting note.
4. Report a summary of everything extracted and where it was filed.

If the user provided text as an argument, process it directly. Otherwise, ask for the transcript or brain dump content.
</process>
