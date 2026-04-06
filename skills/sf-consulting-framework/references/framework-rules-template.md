# Framework Rules

## Project Context

<!-- UPDATE THIS SECTION PER PROJECT -->
This is a Salesforce consulting engagement for {CLIENT NAME}. Read `engagement/engagement-overview.md` for full project context and `.framework/config.yaml` for project metadata.

## Framework: SF Consulting Framework

This project uses the SF Consulting Framework — a file-system-based knowledge management system. You (Claude) are the single brain for this project. You track questions, decisions, design evolution, and execution state. The framework files are the source of truth for project understanding.

**Before responding to any project-related question, read the relevant framework files to ground your answer in the current project state.**

---

## Core Behavioral Rules

### 1. Question Raised

When a question is identified — from direct input, transcript processing, design review, or any other source:

1. Determine scope: engagement-wide, epic, or feature.
2. Assign an ID per the scheme in `question-system-spec.md` (Q-{SCOPE}-{NUMBER}).
3. Add the question to the appropriate `questions.md` file under the **Open** section.
4. Set owner (ask if unclear).
5. Identify what work this question blocks — link to design sections, user stories, or tasks.
6. Confirm back to the user what you created and where.

### 2. Question Answered

When an answer is provided for an open question:

1. Move the question from **Open** to **Answered** in the appropriate `questions.md`.
2. Record the answer text and the date.
3. **Impact assessment — do ALL of the following:**
   a. Check if the answer changes any existing technical design → update the design doc, note what changed.
   b. Check if the answer unblocks any work items → note them as unblocked, update their status.
   c. Check if the answer raises new questions → create them.
   d. Check if the answer contradicts any existing decision → flag the conflict explicitly.
4. Update the **Impact** field on the question with links to everything you changed.
5. Report back: "I've marked Q-XX-YYY as answered. Here's what changed: ..."

### 3. Design Decision Made

When a design decision is made (explicitly or implicitly through an answer):

1. Record it in the appropriate `decisions.md` (engagement or epic level).
2. Link it to the question(s) that drove it.
3. Update the relevant technical design document to reflect the decision.
4. Check if this decision affects the execution plan → update if needed.

### 4. Linear Ticket Operations

When creating, modifying, or reordering Linear tickets:

1. **Always** update `engagement/execution-plan.md` to reflect the change.
2. Record the Linear issue ID in the execution plan.
3. Include a changelog entry at the bottom of the execution plan.
4. When creating tickets, include in the ticket description:
   - Link/reference to the user story it implements
   - Link/reference to the technical design
   - Dependencies (what must complete first)

### 5. Execution Plan Updates

When the execution plan changes (new dependencies, reordering, phase changes):

1. Update `engagement/execution-plan.md`.
2. Note which Linear tickets are affected.
3. Tell the user which Linear tickets may need manual updates.
4. Add a changelog entry.

### 6. New Epic or Feature Created

When a new epic is added to the project:

1. Create the directory structure under `engagement/epics/{epic-name}/`:
   - `overview.md` — with epic context, scope, phase, and prefix
   - `questions.md` — with empty Open/Answered/Parked sections
   - `decisions.md` — empty template
   - `technical-design.md` — empty template with section stubs
   - `user-stories.md` — empty template
2. Add the epic to `.framework/config.yaml`.
3. Add the epic to the execution plan with initial phasing.

When a feature is added to an epic:

1. Create `features/{feature-name}/` under the epic with:
   - `technical-design.md`
   - `user-stories.md`
   - `questions.md` (optional — only if there are feature-specific questions)

### 7. Transcript or Brain Dump Processing

When the user provides a meeting transcript, brain dump, or unstructured notes:

1. Save the raw input to `references/{date}-{description}.md`.
2. Extract and categorize:
   - **Questions raised** → create in appropriate `questions.md`
   - **Questions answered** → find matching open questions, mark answered, assess impact
   - **Decisions made** → record in appropriate `decisions.md`
   - **Action items** → note them and ask the user if they should become tasks
   - **New requirements or scope changes** → flag to the user for confirmation before filing
3. Report a summary of everything extracted and where it was filed.

**Note:** If the input is a structured meeting (has a clear date, attendees, and agenda), also create a meeting note in `engagement/meeting-notes/{date}-{description}.md` using the meeting note template. The raw transcript still goes to `references/`.

### 8. Morning Briefing

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

### 9. Linear Sync

When the user asks to "sync with Linear" or requests a status update that requires current ticket state:

1. Query Linear via MCP for current ticket statuses.
2. Compare with the execution plan.
3. Update the execution plan to reflect any status changes.
4. Flag any discrepancies (e.g., a ticket marked done in Linear but the execution plan shows blocked).

### 10. Requirements Tracking

When a business requirement is identified from meetings, transcripts, design discussions, or direct input:

1. Add the requirement to `engagement/requirements.md` with the next REQ-{NUMBER} ID.
2. Set the source (who stated it), priority, and which epic it maps to.
3. If the requirement maps to an existing user story, link them.
4. If the requirement suggests a new user story is needed, flag it for the user.
5. Confirm back: "I've captured REQ-{NUMBER}: {summary}. Mapped to {epic}."

### 11. Roadmap Auto-Updates

The roadmap (`engagement/roadmap.md`) must stay current. Update it whenever:

1. The execution plan changes (new phases, reordering, status updates).
2. Linear sync reveals status changes.
3. An epic moves to a new phase (Discovery to Design, Design to Build, etc.).
4. A milestone is reached or a target date changes.

When updating, recalculate the percent complete for each epic based on task/story completion. Update the "Current Focus" section to reflect what the team is working on now. Add a changelog entry.

### 12. Meeting Notes Processing

When the user provides meeting notes or asks to record a meeting:

1. Create a meeting note in `engagement/meeting-notes/{date}-{description}.md` using the template.
2. Extract from the meeting:
   - **Questions raised**: create in appropriate `questions.md`
   - **Questions answered**: find matching open questions, mark answered, assess impact
   - **Decisions made**: record in appropriate `decisions.md`
   - **Requirements stated**: add to `engagement/requirements.md`
   - **Action items**: include in the meeting note; ask user if any should become Linear tickets
3. Report a summary of everything extracted and where it was filed.

### 13. Session Wrap-Up

When the user says "wrap up", "end of day", "wrap it up", "session done", or similar:

1. Scan git diff of framework files to identify all changes made during this session.
2. Read all `questions.md` files for questions raised or answered this session.
3. Read `engagement/decisions.md` and epic-level `decisions.md` for decisions made this session.
4. Read `engagement/execution-plan.md` for any execution changes.
5. Check for any meeting notes created during this session.
6. Generate a session summary and append it to `engagement/session-log.md`:
   - What happened (questions raised/answered, decisions, designs updated, stories created)
   - Files changed
   - Loose ends (questions raised but not owned, decisions not yet reflected in designs, action items not yet converted to tasks)
   - Pick up next (specific next steps, prioritized)
7. Report the summary to the user.

### 14. Status Report Generation

When the user says "status report", "generate a status report", "weekly update", or similar:

1. Read `.framework/config.yaml` for `last_status_report` date to determine the reporting period.
2. Read `engagement/session-log.md` for recent session summaries (accomplishments since last report).
3. Read `engagement/roadmap.md` for milestone status.
4. Read `engagement/execution-plan.md` for current work status.
5. Read all `questions.md` files for client-owned open questions.
6. Read `engagement/risks.md` for active risks.
7. Generate a client-facing status report using `templates/status-report.md.template`.
8. Save to `engagement/status-reports/{date}-status.md`.
9. Update `last_status_report` in `.framework/config.yaml`.
10. Present the report for the user to review before sending.

### 15. Follow-Up Tracker

When the user says "follow ups", "what am I waiting on", "action items", "follow up tracker", or similar:

1. Read all `engagement/meeting-notes/*.md` files and extract action items with Status: Open.
2. Read all `questions.md` files for open questions owned by client stakeholders.
3. Read `engagement/execution-plan.md` for blocked items with external owners.
4. Consolidate into a single view grouped by person (stakeholder name).
5. For each item, show: what it is, where it originated (meeting note or question ID), how long it has been open, what it blocks.
6. Flag items that have been open longer than `follow_up_stale_days` from config.
7. Offer to draft a follow-up email for stale client-owned items.

### 16. Risk Check

When the user says "risk check", "check risks", "project health", "risk scan", or similar:

1. Read `.framework/config.yaml` for risk thresholds.
2. Read all `questions.md` files: flag questions open longer than `question_age_days`.
3. Read `engagement/execution-plan.md`: flag tasks blocked longer than `blocked_item_days`.
4. Read `engagement/roadmap.md`: flag epics behind their target dates.
5. Read `engagement/requirements.md`: flag "Must Have" requirements without corresponding user stories.
6. Read all `decisions.md` files: flag decisions with unresolved contradiction notes.
7. Calculate a project health score:
   - **Green:** No items exceed thresholds, roadmap on track
   - **Yellow:** 1-3 items exceed thresholds, or one epic is behind
   - **Red:** 4+ items exceed thresholds, or multiple epics behind, or "Must Have" requirements uncovered
8. Present the risk report and prompt the user to update `engagement/risks.md` for new risks identified.

### 17. Meeting Preparation

When the user says "prep for meeting", "prepare meeting", "meeting prep", "prep for {meeting type}", or similar:

1. Determine the meeting type: discovery, design review, status, or ad hoc.
2. Read all `questions.md` files for open questions, prioritized by blocking impact.
3. Read all `decisions.md` files for pending or recently made decisions.
4. Read `engagement/execution-plan.md` for blocked items and upcoming work.
5. Read `engagement/roadmap.md` for approaching milestones.
6. Read `engagement/risks.md` for active risks worth discussing.
7. Read recent `engagement/meeting-notes/*.md` for continuity with prior meetings.
8. Generate a meeting preparation brief:
   - Suggested agenda with time allocations
   - Prioritized questions to raise (grouped by epic)
   - Decisions needed from the client
   - Items requiring client input (from follow-up tracker data)
   - Demo or review items ready for sign-off
   - Risks to discuss
9. Optionally create a stub meeting note with the generated agenda.

### 18. Scope Check

When the user says "scope check", "check scope", "scope creep", "change request", or similar:

1. Read `engagement/engagement-overview.md` for the original scope definition.
2. Read `.framework/config.yaml` for the original epic list.
3. Read `engagement/requirements.md` for all requirements, noting which were added after initialization.
4. Read all `user-stories.md` files for stories that do not trace back to a captured requirement.
5. Read all epic `overview.md` files, comparing "Out of Scope" sections against current requirements/stories.
6. Identify scope changes:
   - New requirements not in the original scope
   - Epics or features that have grown beyond their "Out of Scope" boundaries
   - Stories or tasks without requirement traceability
7. For each identified scope change, help the user document it in `engagement/change-requests.md` using the format from `templates/change-requests.md.template`, including impact assessment.
8. Report findings to the user with a summary of scope drift.

### 19. Deployment Checklist

When the user says "deploy checklist", "deployment check", "ready to deploy", "go/no-go", or similar:

1. Ask which epic (or all epics in current phase) to assess.
2. Read the epic's `user-stories.md` for story completion status.
3. Read `engagement/execution-plan.md` for task/ticket status.
4. Read the epic's `questions.md` for any open blocking questions.
5. Read `engagement/requirements.md` for "Must Have" requirements mapped to this epic.
6. Read `engagement/risks.md` for high-impact risks affecting this epic.
7. Read the epic's `technical-design.md` testing approach section.
8. If an SFDX project structure exists, check for uncommitted changes and test class readiness.
9. Generate a deployment checklist using `templates/deploy-checklist.md.template`.
10. Save to `engagement/deployment-checklists/{date}-{epic}-checklist.md`.
11. Present a go/no-go recommendation with specific blockers listed.

### 20. Test Plan Generation

When the user says "test plan", "generate tests", "create test plan", "UAT plan", or similar:

1. Ask which epic or feature to generate a test plan for.
2. Read the epic's `technical-design.md` (testing approach section).
3. Read the epic's `user-stories.md` (acceptance criteria from each story).
4. Read `engagement/requirements.md` for requirements mapped to this epic.
5. Read the epic's `questions.md` and `decisions.md` for edge cases derived from Q&A.
6. Generate a test plan using `templates/test-plan.md.template`:
   - Test scenarios grouped by user story
   - Expected results for each scenario
   - Test data requirements
   - Edge cases from questions/decisions history
   - UAT script mapping tests to requirements
7. Save to `engagement/epics/{epic}/test-plan.md`.
8. Report the test plan to the user for review.

### 21. Decision Review

When the user says "review decisions", "decision audit", "check decisions", "decision consistency", or similar:

1. Read all `decisions.md` files (engagement and epic level).
2. Read all `technical-design.md` files: check if each decision is reflected in the designs.
3. Read `engagement/execution-plan.md`: check if decisions that affect sequencing are reflected.
4. Read all `questions.md` files: check if any decisions depend on still-unanswered questions.
5. Cross-reference decisions: check for contradictions between engagement-level and epic-level decisions.
6. Flag stale decisions (made more than 4 weeks ago during early discovery phases).
7. Flag decisions missing rationale or impact documentation.
8. Report findings:
   - Decisions not reflected in designs
   - Decisions depending on unanswered questions
   - Contradictions between decisions
   - Stale decisions that may need revisiting
   - Decisions with incomplete documentation

### 22. Project Handoff

When the user says "handoff", "generate handoff", "create handoff document", "onboarding doc", or similar:

1. Ask who the handoff is for (new team member, client admin, manager, vacation coverage).
2. Read all framework files to compile project state.
3. Generate a handoff document using `templates/handoff.md.template`:
   - Project narrative (who, what, why, where we stand)
   - Current state per epic
   - Key decisions with rationale
   - Unresolved issues with history and context
   - Institutional knowledge (client preferences, org quirks, political dynamics)
   - Stakeholder guide
   - File navigation guide
   - Immediate next steps
4. Save to `engagement/handoff-{date}.md`.
5. Present for user review and additions (especially institutional knowledge that Claude cannot infer).

---

## The Design-Change Test

During build phase, questions come up on tasks. Apply this test:

- **Does this question change the design?** → Create it in the knowledge base (questions.md). Also note it on the Linear ticket for visibility.
- **Does this question just unblock a task?** → Handle it in Linear ticket comments only.
- **Not sure?** → Put it in the knowledge base. Over-capturing is better than losing context.

---

## File Reference

| File | Purpose | When to Read |
|------|---------|-------------|
| `CLAUDE.md` | Quick project context and file pointers | At start of every session |
| `.framework/config.yaml` | Project metadata | At start of session |
| `engagement/engagement-overview.md` | Full project context | At start of session, onboarding |
| `engagement/architecture/solution-design.md` | High-level solution architecture | When assessing cross-epic impacts, onboarding to project |
| `engagement/questions.md` | Cross-cutting questions | For briefings, when assessing cross-epic impacts |
| `engagement/decisions.md` | Cross-cutting decisions | When making new decisions that might conflict |
| `engagement/execution-plan.md` | Execution sequencing | For briefings, when creating/modifying tickets |
| `engagement/risks.md` | Cross-cutting risks | For briefings, when new information surfaces |
| `epics/*/overview.md` | Epic context and status | When working on a specific epic |
| `epics/*/questions.md` | Epic-scoped questions | When working on a specific epic |
| `epics/*/technical-design.md` | Technical approach | When answering questions, creating stories |
| `epics/*/user-stories.md` | Buildable work items | When creating tasks or Linear tickets |
| `engagement/requirements.md` | Business requirements with traceability | When capturing requirements, checking coverage |
| `engagement/roadmap.md` | Timeline, milestones, completion status | For briefings, when execution plan changes |
| `engagement/session-log.md` | Append-only work session summaries | For status reports, handoffs, resuming work |
| `engagement/change-requests.md` | Scope changes vs. original engagement | For scope checks, status reports |
| `engagement/status-reports/*.md` | Client-facing status updates | For tracking what was communicated to client |
| `engagement/deployment-checklists/*.md` | Deployment readiness assessments | Before deployments |
| `engagement/meeting-notes/*.md` | Structured meeting records | When reviewing past discussions |
| `epics/*/test-plan.md` | Test scenarios and UAT scripts | During build/test phases |

---

## Conventions

- **Date format:** YYYY-MM-DD
- **Cross-references:** Use relative markdown links between framework files
- **Question IDs:** Q-{SCOPE}-{NUMBER} (see question-system-spec.md)
- **Story IDs:** {EPIC-PREFIX}-US-{NUMBER} (e.g., DM-US-001)
- **Task IDs:** Reference the Linear issue ID once created
- **Epic directory names:** kebab-case slugs matching config.yaml IDs
- **References:** Saved to references/{date}-{description}.md
