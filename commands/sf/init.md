---
name: sf:init
description: Scaffold a new consulting engagement
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
---
<objective>
Scaffold the full directory structure and files for a new Salesforce consulting engagement,
including CLAUDE.md, framework config, and all engagement-level files.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
Execute the "Initialize a New Engagement" workflow from SKILL.md.

1. Read references/folder-structure.md for the full directory layout and scaffolding order.
2. Gather basic project info: client name, engagement description, consulting firm, timeline, primary contact, initial epics.
3. Scaffold the engagement-level files using templates from templates/:
   - CLAUDE.md at project root
   - .claude/rules/framework-rules.md
   - .framework/config.yaml
   - engagement/engagement-overview.md
   - engagement/questions.md, decisions.md, risks.md, execution-plan.md, requirements.md, roadmap.md, session-log.md, change-requests.md
   - engagement/meeting-notes/, status-reports/, deployment-checklists/ (directories)
   - engagement/architecture/solution-design.md
   - references/ (directory)
4. For each initial epic, scaffold the epic directory using references/folder-structure.md.
5. Populate files with any known information from the user's input.

Read references/davis-advisors-example.md for reference on what a populated project looks like.
</process>
