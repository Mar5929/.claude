---
name: sf:risks
description: Project health and risk scan
allowed-tools:
  - Read
  - Bash
  - Grep
  - Glob
---
<objective>
Scan the project for risk signals and calculate a project health score (Green/Yellow/Red)
based on configurable thresholds.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
Execute the "Risk Check" workflow from SKILL.md.

1. Read .framework/config.yaml for risk thresholds.
2. Scan for risk signals:
   - Questions open longer than question_age_days (default 7)
   - Client-owned questions with no activity past follow_up_stale_days (default 3)
   - Tasks blocked longer than blocked_item_days (default 5)
   - Epics behind their roadmap target dates
   - "Must Have" requirements without corresponding user stories
   - Decisions with unresolved contradiction notes
3. Calculate project health score:
   - Green: No items exceed thresholds, roadmap on track
   - Yellow: 1-3 items exceed thresholds, or one epic is behind
   - Red: 4+ items exceed thresholds, multiple epics behind, or "Must Have" requirements uncovered
4. Present the risk report and prompt user to update engagement/risks.md.
</process>
