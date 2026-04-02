---
name: sf:tickets
description: Create or manage Linear tickets from the execution plan
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
  - Agent
---
<objective>
Create, modify, or reorder Linear tickets based on the execution plan, keeping
the execution plan and Linear in sync.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
Execute the "Linear Ticket Operations" workflow from SKILL.md.

1. Read references/execution-plan.md for Linear sync conventions.
2. Always update engagement/execution-plan.md to reflect the change.
3. Record the Linear issue ID in the execution plan.
4. Include a changelog entry at the bottom of the execution plan.
5. When creating tickets, include in the ticket description:
   - Link/reference to the user story it implements
   - Link/reference to the technical design
   - Dependencies (what must complete first)

Use the Linear MCP tools (save_issue, list_issues, etc.) for ticket operations.
Follow the Linear rules: assign to Michael Rihm, use team Rihm (RIH).
</process>
