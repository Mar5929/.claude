---
name: sf:briefing
description: Get project status and recommended focus
allowed-tools:
  - Read
  - Bash
  - Grep
  - Glob
---
<objective>
Generate a morning briefing covering open questions, blocked work, execution status,
roadmap progress, requirements gaps, and recommended focus areas.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
Execute the "Morning Briefing" workflow from SKILL.md.

1. Read all questions.md files for open question state.
2. Read engagement/execution-plan.md for execution state.
3. Read epic overview.md files for phase status.
4. Read engagement/roadmap.md for milestone and timeline status.
5. Read engagement/requirements.md for unvalidated requirements.
6. Synthesize into a briefing covering:
   - Open questions: count by scope, owners, which block work
   - Blocked work: what's waiting and on what
   - Recently resolved: answered/unblocked since last briefing
   - Execution status: in progress, next up, critical path
   - Roadmap progress: milestone status, percent complete
   - Requirements gaps: captured but not mapped to epics/stories
   - Recommended focus: 2-3 prioritized items to work on now
</process>
