---
name: sf-consulting-framework
description: >-
  Salesforce consulting engagement knowledge management framework. Turns any SF consulting
  project repo into a structured knowledge base with question tracking, design hierarchy,
  and execution planning. Use when the user says "initialize a new engagement",
  "initialize a new project", "scaffold a project", "what's next", "give me a briefing",
  "morning briefing", "where do we stand", "sync with Linear", "process this transcript",
  "process this brain dump", "raise a question", "answer a question", "create user stories",
  "create tasks", "create Linear tickets", "scaffold a new epic", "add an epic",
  "add a feature", "update the execution plan", "wrap up", "end of day", "session done",
  "status report", "weekly update", "follow ups", "what am I waiting on", "action items",
  "risk check", "project health", "risk scan", "prep for meeting", "meeting prep",
  "scope check", "scope creep", "change request", "deploy checklist", "ready to deploy",
  "go/no-go", "test plan", "UAT plan", "generate tests", "review decisions",
  "decision audit", "handoff", "create handoff document", "onboarding doc",
  or any Salesforce consulting project management or knowledge tracking activity.
  Also trigger when a project's CLAUDE.md references the SF Consulting Framework or
  when .framework/config.yaml exists.
  Do NOT trigger for Salesforce technical solutioning (use sf-architect-solutioning instead)
  or Salesforce development patterns (use sf-develop instead). Do NOT trigger for general
  Salesforce development tasks that don't involve project management or knowledge tracking.
allowed-tools: Bash, Write, Read, Edit, Glob, Grep, Agent
---

# SF Consulting Framework

A file-system-based knowledge management framework for Salesforce consulting engagements. Claude acts as the project's single brain: tracking questions, decisions, design evolution, and execution state across the full lifecycle.

This is not a project management tool. It is a **living knowledge base with project management behaviors**. PM tools track work items. This system tracks *understanding*, and work items fall out of that naturally.

## Three Integrated Systems

### 1. Knowledge Base (Discovery Engine)

Tracks questions, answers, and decisions. Questions are the atomic unit of discovery. When an answer comes in, Claude fans out the impact: updates designs, flags unblocked work, surfaces new questions.

You only need to be consistent at one point: telling Claude when an answer comes in. Claude handles all downstream consistency.

Validated requirements are tracked in `engagement/requirements.md`, bridging questions (unknowns) and technical designs (solutions).

**Detailed spec:** Read `references/question-system.md`

### 2. Design Hierarchy (Progressive Decomposition)

Structured breakdown mirrored in the folder structure:

| Level | Description |
|-------|-------------|
| Engagement | The whole project + cross-cutting concerns |
| Epic | Major workstream / scope item |
| Feature *(optional)* | Logical grouping within a complex epic |
| Technical Design | How a feature or epic will be built (generates user stories) |
| User Story | A buildable unit of work (generates tasks) |
| Task | Most granular unit (becomes a Linear ticket) |

**Detailed spec:** Read `references/folder-structure.md`

### 3. Execution Plan (Sequencing & Dependencies)

Dependency graph and phase ordering. Defines what goes first, what's parallel, what's blocked. Bridges the design hierarchy and Linear.

Traceability is embedded in conventions: every question references what it blocks, every decision references what drove it, every story references its design doc.

**Detailed spec:** Read `references/execution-plan.md`

---

## Core Workflows

### Initialize a New Engagement

When the user asks to initialize a new project or engagement:

1. Read `references/folder-structure.md` for the full directory layout and scaffolding order.
2. Gather basic project info: client name, engagement description, consulting firm, timeline, primary contact, initial epics.
3. Scaffold the engagement-level files using templates from `templates/`:
   - `CLAUDE.md` at project root (from `templates/claude-md.md.template`)
   - `.claude/rules/framework-rules.md` (from `references/framework-rules-template.md`)
   - `.framework/config.yaml` (from `templates/config.yaml.template`)
   - `engagement/engagement-overview.md` (from `templates/engagement-overview.md.template`)
   - `engagement/questions.md` (from `templates/questions.md.template`)
   - `engagement/decisions.md` (from `templates/decisions.md.template`)
   - `engagement/risks.md` (from `templates/risks.md.template`)
   - `engagement/execution-plan.md` (from `templates/execution-plan.md.template`)
   - `engagement/requirements.md` (from `templates/requirements.md.template`)
   - `engagement/roadmap.md` (from `templates/roadmap.md.template`)
   - `engagement/session-log.md` (from `templates/session-log.md.template`)
   - `engagement/change-requests.md` (from `templates/change-requests.md.template`)
   - `engagement/meeting-notes/` (empty directory)
   - `engagement/status-reports/` (empty directory)
   - `engagement/deployment-checklists/` (empty directory)
   - `engagement/architecture/solution-design.md` (from `templates/solution-design.md.template`)
   - `references/` (empty directory)
   - `dashboard/server.js` (from `templates/dashboard/server.js.template`)
   - `dashboard/index.html` (from `templates/dashboard/index.html.template`)
   - `dashboard/package.json` (from `templates/dashboard/package.json.template`)
   - `dashboard/.gitignore` (containing `node_modules/`)
4. For each initial epic, scaffold the epic directory using the epic scaffolding order in `references/folder-structure.md`.
5. Populate files with any known information from the user's input.

**Example:** Read `references/davis-advisors-example.md` for a complete example of what a populated project looks like.

### Question Raised

When a question is identified from any source (direct input, transcript, design review):

1. Read `references/question-system.md` for the full question lifecycle and ID scheme.
2. Determine scope: engagement-wide, epic, or feature.
3. Assign an ID: `Q-{SCOPE}-{NUMBER}` (e.g., Q-ENG-001, Q-DM-003).
4. Add the question to the appropriate `questions.md` file under the **Open** section.
5. Set owner (ask if unclear).
6. Identify what work this question blocks and link to design sections, user stories, or tasks.
7. Confirm back to the user what you created and where.

### Question Answered

When an answer is provided for an open question:

1. Move the question from **Open** to **Answered** in the appropriate `questions.md`.
2. Record the answer text and the date.
3. **Impact assessment (do ALL of the following):**
   a. Check if the answer changes any existing technical design. If yes, update the design doc and note what changed.
   b. Check if the answer unblocks any work items. If yes, note them as unblocked and update their status.
   c. Check if the answer raises new questions. If yes, create them.
   d. Check if the answer contradicts any existing decision. If yes, flag the conflict explicitly.
4. Update the **Impact** field on the question with links to everything changed.
5. Report back: "I've marked Q-XX-YYY as answered. Here's what changed: ..."

### Requirement Captured

When a business requirement is identified from meetings, transcripts, design discussions, or direct input:

1. Add the requirement to `engagement/requirements.md` with the next `REQ-{NUMBER}` ID.
2. Set the source (who stated it), priority, and which epic it maps to.
3. If the requirement maps to an existing user story, link them.
4. If the requirement suggests a new user story is needed, flag it for the user.
5. Confirm back: "I've captured REQ-{NUMBER}: {summary}. Mapped to {epic}."

### Morning Briefing

When the user asks "what's next?", "give me a briefing," "where do we stand?", or similar:

1. Read all `questions.md` files to get the open question state.
2. Read `engagement/execution-plan.md` for execution state.
3. Read epic `overview.md` files for phase status.
4. Read `engagement/roadmap.md` for milestone and timeline status.
5. Read `engagement/requirements.md` to check for unvalidated requirements.
6. Synthesize into a briefing covering:
   - **Open questions**: count by scope, who owns them, which are blocking work
   - **Blocked work**: what's waiting and on what
   - **Recently resolved**: what got answered/unblocked since last briefing
   - **Execution status**: what's in progress, what's next, critical path
   - **Roadmap progress**: milestone status and percent complete
   - **Requirements gaps**: any captured requirements not yet mapped to epics or stories
   - **Recommended focus**: the 2-3 things to work on now, prioritized

### Transcript or Brain Dump Processing

When the user provides a meeting transcript, brain dump, or unstructured notes:

1. Save the raw input to `references/{date}-{description}.md`.
2. Extract and categorize:
   - **Questions raised**: create in appropriate `questions.md`
   - **Questions answered**: find matching open questions, mark answered, assess impact
   - **Decisions made**: record in appropriate `decisions.md`
   - **Requirements stated**: add to `engagement/requirements.md`
   - **Action items**: note them and ask the user if they should become tasks
   - **New requirements or scope changes**: flag to the user for confirmation before filing
3. Report a summary of everything extracted and where it was filed.
4. If the input is a structured meeting (has a clear date, attendees, and agenda), also create a meeting note in `engagement/meeting-notes/{date}-{description}.md` using `templates/meeting-note.md.template`.

### Meeting Notes Processing

When the user provides meeting notes or asks to record a meeting:

1. Create a meeting note in `engagement/meeting-notes/{date}-{description}.md` using `templates/meeting-note.md.template`.
2. Extract questions, answers, decisions, requirements, and action items into the appropriate framework files.
3. Report a summary of everything extracted and where it was filed.

### Design Decision Made

When a design decision is made (explicitly or implicitly through an answer):

1. Record it in the appropriate `decisions.md` (engagement or epic level).
2. Link it to the question(s) that drove it.
3. Update the relevant technical design document to reflect the decision.
4. Check if this decision affects the execution plan and update if needed.

### Linear Ticket Operations

When creating, modifying, or reordering Linear tickets:

1. Read `references/execution-plan.md` for Linear sync conventions.
2. **Always** update `engagement/execution-plan.md` to reflect the change.
3. Record the Linear issue ID in the execution plan.
4. Include a changelog entry at the bottom of the execution plan.
5. When creating tickets, include in the ticket description:
   - Link/reference to the user story it implements
   - Link/reference to the technical design
   - Dependencies (what must complete first)

### Linear Sync

When the user asks to "sync with Linear" or requests a status update:

1. Query Linear via MCP for current ticket statuses.
2. Compare with the execution plan.
3. Update the execution plan to reflect any status changes.
4. Flag any discrepancies (e.g., a ticket marked done in Linear but the execution plan shows blocked).

### Roadmap Auto-Updates

The roadmap (`engagement/roadmap.md`) must stay current. Update it whenever:

1. The execution plan changes (new phases, reordering, status updates).
2. Linear sync reveals status changes.
3. An epic moves to a new phase (Discovery to Design, Design to Build, etc.).
4. A milestone is reached or a target date changes.

When updating, recalculate the percent complete for each epic based on task/story completion. Update the "Current Focus" section. Add a changelog entry.

### Session Wrap-Up

When the user says "wrap up", "end of day", "session done", or similar:

1. Use `git diff` on framework files to identify all changes made during this session.
2. Read all `questions.md` files for questions raised or answered this session.
3. Read `engagement/decisions.md` and epic-level `decisions.md` for decisions made.
4. Read `engagement/execution-plan.md` for execution changes.
5. Check `engagement/meeting-notes/` for any notes created this session.
6. Generate a session summary and append to `engagement/session-log.md`:
   - **What Happened:** questions raised/answered, decisions, designs updated, stories created
   - **Files Changed:** list with what changed in each
   - **Loose Ends:** questions raised but not owned, decisions not reflected in designs, action items not converted to tasks
   - **Pick Up Next:** specific next steps, prioritized
7. Report the summary to the user.

### Status Report Generation

When the user says "status report", "weekly update", or similar:

1. Read `.framework/config.yaml` for `last_status_report` date to determine the reporting period.
2. Read `engagement/session-log.md` for recent accomplishments.
3. Read `engagement/roadmap.md` for milestone status.
4. Read `engagement/execution-plan.md` for current work status.
5. Read all `questions.md` files for client-owned open questions.
6. Read `engagement/risks.md` for active risks.
7. Generate a client-facing status report using `templates/status-report.md.template`:
   - Translate internal framework jargon into plain business language
   - Group "Items Needing Client Input" by tying them to specific open questions the client owns
   - Include milestone status with on-track/at-risk/delayed assessment
8. Save to `engagement/status-reports/{date}-status.md`.
9. Update `last_status_report` in `.framework/config.yaml`.
10. Present the report for user review before sending.

### Follow-Up Tracker

When the user says "follow ups", "what am I waiting on", "action items", or similar:

1. Read all `engagement/meeting-notes/*.md` and extract action items with Status: Open.
2. Read all `questions.md` files for open questions owned by client stakeholders.
3. Read `engagement/execution-plan.md` for blocked items with external owners.
4. Read `.framework/config.yaml` for `follow_up_stale_days` threshold.
5. Consolidate into a single view grouped by person (stakeholder name):
   - What the item is
   - Where it originated (meeting note filename or question ID)
   - How long it has been open
   - What it blocks
6. Flag items exceeding `follow_up_stale_days`.
7. Offer to draft a follow-up email for stale client-owned items.

### Risk Check

When the user says "risk check", "project health", "risk scan", or similar:

1. Read `.framework/config.yaml` for risk thresholds.
2. Scan for risk signals:
   - Questions open longer than `question_age_days` (default 7)
   - Client-owned questions with no activity past `follow_up_stale_days` (default 3)
   - Tasks blocked longer than `blocked_item_days` (default 5)
   - Epics behind their roadmap target dates
   - "Must Have" requirements without corresponding user stories
   - Decisions with unresolved contradiction notes
3. Calculate project health score:
   - **Green:** No items exceed thresholds, roadmap on track
   - **Yellow:** 1-3 items exceed thresholds, or one epic is behind
   - **Red:** 4+ items exceed thresholds, multiple epics behind, or "Must Have" requirements uncovered
4. Present the risk report and prompt user to update `engagement/risks.md`.

### Meeting Preparation

When the user says "prep for meeting", "meeting prep", or similar:

1. Determine meeting type: discovery, design review, status, or ad hoc.
2. Read all `questions.md` files for open questions, prioritized by blocking impact.
3. Read all `decisions.md` for pending or recently made decisions.
4. Read `engagement/execution-plan.md` for blocked items and upcoming work.
5. Read `engagement/roadmap.md` for approaching milestones.
6. Read `engagement/risks.md` for active risks.
7. Read recent `engagement/meeting-notes/*.md` for continuity.
8. Generate a meeting preparation brief:
   - Suggested agenda with time allocations
   - Prioritized questions to raise (grouped by epic)
   - Decisions needed from the client
   - Items requiring client input
   - Demo or review items ready for sign-off
   - Risks to discuss
9. Optionally create a stub meeting note with the generated agenda.

### Scope Check

When the user says "scope check", "scope creep", "change request", or similar:

1. Read `engagement/engagement-overview.md` for original scope.
2. Read `.framework/config.yaml` for original epic list.
3. Read `engagement/requirements.md` for requirements added after initialization.
4. Read all `user-stories.md` for stories without requirement traceability.
5. Read all epic `overview.md` "Out of Scope" sections against current work.
6. Identify scope changes:
   - New requirements not in original scope
   - Epics grown beyond their "Out of Scope" boundaries
   - Stories or tasks without requirement traceability
7. For each change, help document in `engagement/change-requests.md` using `templates/change-requests.md.template`.
8. Report scope drift summary.

### Deployment Checklist

When the user says "deploy checklist", "ready to deploy", "go/no-go", or similar:

1. Ask which epic (or all in current phase) to assess.
2. Read the epic's `user-stories.md` for completion status.
3. Read `engagement/execution-plan.md` for task/ticket status.
4. Read the epic's `questions.md` for open blockers.
5. Read `engagement/requirements.md` for "Must Have" coverage.
6. Read `engagement/risks.md` for high-impact risks.
7. Read the epic's `technical-design.md` testing approach section.
8. If SFDX project exists, check for uncommitted changes and test coverage.
9. Generate checklist using `templates/deploy-checklist.md.template`.
10. Save to `engagement/deployment-checklists/{date}-{epic}-checklist.md`.
11. Present go/no-go recommendation with specific blockers.

### Test Plan Generation

When the user says "test plan", "UAT plan", "generate tests", or similar:

1. Ask which epic or feature to generate a test plan for.
2. Read the epic's `technical-design.md` (testing approach section).
3. Read the epic's `user-stories.md` (acceptance criteria).
4. Read `engagement/requirements.md` for requirements mapped to this epic.
5. Read the epic's `questions.md` and `decisions.md` for edge cases from Q&A.
6. Generate test plan using `templates/test-plan.md.template`:
   - Test scenarios grouped by user story
   - Expected results
   - Test data requirements
   - Edge cases derived from questions/decisions
   - UAT script mapping tests to requirements
7. Save to `engagement/architecture/epics/{epic}/test-plan.md`.

### Decision Review

When the user says "review decisions", "decision audit", or similar:

1. Read all `decisions.md` files (engagement and epic level).
2. Cross-reference against:
   - `technical-design.md` files: is each decision reflected?
   - `execution-plan.md`: did sequencing-affecting decisions get updated?
   - `questions.md` files: do any decisions depend on unanswered questions?
   - Other decisions: contradictions between engagement and epic level?
3. Flag stale decisions (4+ weeks old from early discovery).
4. Flag decisions missing rationale or impact documentation.
5. Report findings with specific items to review.

### Project Handoff

When the user says "handoff", "create handoff document", "onboarding doc", or similar:

1. Ask who the handoff is for (new team member, client admin, manager, vacation coverage).
2. Read all framework files to compile project state.
3. Generate using `templates/handoff.md.template`:
   - Project narrative (who, what, why, current state)
   - Current state per epic with key context
   - Key decisions with rationale
   - Unresolved issues with history
   - Institutional knowledge (ask user to fill in: client preferences, org quirks, political dynamics)
   - Stakeholder guide
   - File navigation guide
   - Immediate next steps
4. Save to `engagement/handoff-{date}.md`.
5. Present for user review and additions.

### Scaffold New Epic

When a new epic is added to the project:

1. Read `references/folder-structure.md` for the epic scaffolding order.
2. Create the directory structure under `engagement/architecture/epics/{epic-name}/`:
   - `overview.md` (from `templates/epic-overview.md.template`)
   - `questions.md` (from `templates/questions.md.template`)
   - `decisions.md` (from `templates/decisions.md.template`)
   - `technical-design.md` (from `templates/technical-design.md.template`)
   - `user-stories.md` (from `templates/user-stories.md.template`)
3. Add the epic to `.framework/config.yaml`.
4. Add the epic to the execution plan with initial phasing.
5. Update `engagement/roadmap.md` with the new epic row.

### Scaffold Feature (Optional)

Only create a `features/` subdirectory when:
- The epic has identifiable distinct functional areas that each need their own design
- Multiple user stories would benefit from being grouped under a named feature
- The technical design is complex enough that a single design doc becomes unwieldy

If in doubt, don't create it. You can always restructure later.

---

## The Design-Change Test

During build phase, questions come up on tasks. Apply this test:

- **Does this question change the design?** Create it in the knowledge base (questions.md). Also note it on the Linear ticket for visibility.
- **Does this question just unblock a task?** Handle it in Linear ticket comments only.
- **Not sure?** Put it in the knowledge base. Over-capturing beats losing context.

---

## What Lives Where

| Information | Primary Home | Why |
|------------|-------------|-----|
| Task sequencing and dependencies | Execution plan (framework file) | Claude needs the full picture |
| Task status | Linear (primary), execution plan (synced) | Linear is where you work; execution plan is where Claude reads |
| Technical context and rationale | Framework files | Too detailed for ticket descriptions |
| Day-to-day implementation notes | Linear ticket comments | Build context that doesn't affect design |
| Design-changing questions from build | Framework questions.md + Linear ticket reference | Preserves "single brain" |
| Task-unblocking questions | Linear ticket only | Doesn't need the knowledge base |

---

## Lifecycle Phases

| Phase | Primary System | Claude's Focus | Companion Skills |
|-------|---------------|----------------|-----------------|
| **Discovery** | Knowledge Base | Heavy question tracking, answer processing, decision recording | sf-org-knowledge (optional: "analyze org" to scan the existing org) |
| **Design** | Design Hierarchy | Formalizing technical designs, generating user stories from designs | sf-architect-solutioning ("architect this" / "solution this requirement") |
| **Build** | Execution Plan | Task creation, Linear tickets, execution sequencing, handling design-change questions | sf-develop (Salesforce metadata, Apex, LWC, and testing guidance) |
| **Test & Deploy** | All three | Issues map to design gaps, deployment decisions, execution tracking | |

---

## Reference Files

Read these for detailed mechanics when performing specific workflows:

| Reference | When to Read |
|-----------|-------------|
| `references/folder-structure.md` | Scaffolding a new project or adding epics/features |
| `references/question-system.md` | Handling questions (raising, answering, processing transcripts) |
| `references/execution-plan.md` | Creating tasks, Linear tickets, or updating the execution plan |
| `references/framework-rules-template.md` | Generating the `.claude/rules/framework-rules.md` for a new project |
| `references/davis-advisors-example.md` | Understanding what a populated project looks like |

## Template Files

All templates live in `templates/`. Use them when scaffolding new projects or adding new files. Replace `{placeholder}` values with actual project data.

| Template | Creates |
|----------|---------|
| `claude-md.md.template` | Root `CLAUDE.md` |
| `config.yaml.template` | `.framework/config.yaml` |
| `engagement-overview.md.template` | `engagement/engagement-overview.md` |
| `questions.md.template` | `questions.md` (engagement or epic level) |
| `decisions.md.template` | `decisions.md` (engagement or epic level) |
| `risks.md.template` | `engagement/risks.md` |
| `execution-plan.md.template` | `engagement/execution-plan.md` |
| `requirements.md.template` | `engagement/requirements.md` |
| `roadmap.md.template` | `engagement/roadmap.md` |
| `meeting-note.md.template` | `engagement/meeting-notes/{date}-{description}.md` |
| `solution-design.md.template` | `engagement/architecture/solution-design.md` |
| `technical-design.md.template` | `technical-design.md` (epic or feature level) |
| `user-stories.md.template` | `user-stories.md` (epic or feature level) |
| `epic-overview.md.template` | Epic `overview.md` |
| `session-log.md.template` | `engagement/session-log.md` |
| `status-report.md.template` | `engagement/status-reports/{date}-status.md` |
| `change-requests.md.template` | `engagement/change-requests.md` |
| `deploy-checklist.md.template` | `engagement/deployment-checklists/{date}-{epic}-checklist.md` |
| `test-plan.md.template` | `engagement/architecture/epics/{epic}/test-plan.md` |
| `handoff.md.template` | `engagement/handoff-{date}.md` |
| `dashboard/server.js.template` | `dashboard/server.js` |
| `dashboard/index.html.template` | `dashboard/index.html` |
| `dashboard/package.json.template` | `dashboard/package.json` |
