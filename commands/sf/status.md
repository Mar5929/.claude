---
name: sf:status
description: Generate a client-facing status report
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
---
<objective>
Generate a client-facing status report covering the reporting period since the last
status report, with milestone progress, blockers, and items needing client input.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
Execute the "Status Report Generation" workflow from SKILL.md.

1. Read .framework/config.yaml for last_status_report date to determine the reporting period.
2. Read engagement/session-log.md for recent accomplishments.
3. Read engagement/roadmap.md for milestone status.
4. Read engagement/execution-plan.md for current work status.
5. Read all questions.md files for client-owned open questions.
6. Read engagement/risks.md for active risks.
7. Generate a client-facing status report using templates/status-report.md.template:
   - Translate internal framework jargon into plain business language
   - Group "Items Needing Client Input" by tying them to specific open questions the client owns
   - Include milestone status with on-track/at-risk/delayed assessment
8. Save to engagement/status-reports/{date}-status.md.
9. Update last_status_report in .framework/config.yaml.
10. Present the report for user review before sending.
</process>
