# SF Consulting Framework — Execution Plan

The execution plan maps the sequence in which work gets built. It defines phase ordering across epics, dependency chains between tasks, parallel work opportunities, and the connection to Linear tickets. It lives in a single file — `engagement/execution-plan.md` — and is the bridge between the design hierarchy and day-to-day execution.

---

## What the Execution Plan Tracks

### Epic Phasing
Which epics start first, which depend on others, and which can run in parallel.

### Feature Sequencing (within an epic)
If an epic has features, which features need to be built first because others depend on them.

### Task Dependencies
At the most granular level, which tasks must complete before other tasks can begin. This includes both within-epic and cross-epic dependencies.

### Linear Ticket References
Every task in the execution plan maps to a Linear ticket. The execution plan records the Linear issue ID so you can trace between the two systems.

---

## File Format

```markdown
# Execution Plan — {Engagement Name}

**Last updated:** 2025-02-10
**Updated by:** Claude (triggered by: created Linear tickets for Epic A)

---

## Epic Phasing

| Phase | Epic | Rationale | Status |
|-------|------|-----------|--------|
| 1 | Data Cleanup | Foundational — clean data before loading more. Other epics depend on clean records. | In Progress — Design |
| 1 | Legacy Audit | Can run in parallel with Cleanup. Audit is read-only, doesn't modify data. | In Progress — Discovery |
| 2 | Data Loads | Depends on Cleanup completing. Loading data into dirty system creates more issues. | Not Started — Blocked by Cleanup |
| 2 | Integration Setup | Can run in parallel with Data Loads. No dependency on Cleanup (different data source). | Not Started — Design ready |

### Phase Dependencies
- **Phase 2 start requires:** Data Cleanup execution complete (not just designed — actually executed)
- **Exception:** Integration Setup can start during Phase 1 since it targets a separate dataset

---

## Feature Sequencing — Legacy Audit

| Order | Feature | Depends On | Linear Epic |
|-------|---------|------------|-------------|
| 1 | Audit Existing Logic | Nothing — first step | LA-EPIC |
| 2 | Complex Transaction Handling | Audit results (#1) — need to know if existing work is salvageable | LA-EPIC |
| 3 | Batch Job Optimization | Complex transaction design (#2) — optimization depends on final logic | LA-EPIC |

---

## Task Execution — Data Cleanup

| Order | Task | Story | Depends On | Linear Issue | Status |
|-------|------|-------|------------|-------------|--------|
| 1 | Query all records with external IDs, identify duplicates | DC-US-001 | — | DC-101 | To Do |
| 2 | Define merge rules for each field | DC-US-001 | #1 (need duplicate analysis to inform rules) | DC-102 | Blocked by Q-DC-001 |
| 3 | Configure merge tool template | DC-US-002 | #2 (merge rules must be defined) | DC-103 | Not Started |
| 4 | Execute test merge on sandbox (50 records) | DC-US-002 | #3 | DC-104 | Not Started |
| 5 | Validate test merge — check segments, reports, rollups | DC-US-003 | #4 | DC-105 | Not Started |
| 6 | Execute full production merge | DC-US-003 | #5 (validation must pass) | DC-106 | Not Started |
| 7 | Post-merge validation and stakeholder sign-off | DC-US-003 | #6 | DC-107 | Not Started |

### Parallel Work Opportunities
- Tasks 1-2 (Data Cleanup) can run in parallel with the entire Legacy Audit epic
- Within an epic, tasks are generally sequential unless noted otherwise

### Cross-Epic Dependencies
- Data Loads (Phase 2) cannot start until Cleanup Task #6 (production merge) is complete
- Legacy Audit redesign (if needed after audit) should incorporate Cleanup results to ensure recalculations account for merged records

---

## Blocked Items

| Item | Blocked By | Type | Owner | Since |
|------|-----------|------|-------|-------|
| DC-102 (merge rules) | Q-DC-001 — conflicting field value handling | Question | Client | 2025-01-20 |
| Legacy Audit Feature #2 start | Feature #1 completion (audit) | Dependency | Us | — |
| Data Loads epic start | Cleanup production merge (DC-106) | Phase dependency | — | — |

---
```

---

## Linear Sync Conventions

### When Claude Creates Linear Tickets

Claude creates Linear tickets when user stories are broken into tasks and you confirm the task breakdown is ready for execution. Claude:

1. Creates the Linear tickets (one per task) with:
   - Title matching the task description
   - Description linking back to the user story and technical design
   - Labels for the epic
   - Priority based on execution order
   - Dependencies noted in the description (not just Linear's built-in blocking — also a plain-text note of what must finish first)

2. Updates the execution plan with:
   - Linear issue IDs in the `Linear Issue` column
   - Status synced from the ticket creation
   - A changelog entry noting what was created

3. Groups tickets under the appropriate Linear epic/project

### When Claude Modifies the Execution Plan

Any change to the execution plan — reordering, new dependencies, status changes — Claude also:

1. Notes which Linear tickets are affected
2. Tells you which tickets need manual updates in Linear (if the change affects ticket descriptions, priorities, or blocking relationships)
3. Records the change in the execution plan changelog

### When You Update Linear Directly

If you update a Linear ticket status outside of Claude (e.g., marking a task complete in Linear's UI), the execution plan will be stale until you tell Claude. The convention:

- At morning briefing time, you can ask Claude to "sync with Linear" — Claude reads the current state of tickets via MCP and updates the execution plan accordingly.
- Or you can tell Claude as you go: "DC-101 is done" and Claude updates both the execution plan and checks if anything is now unblocked.

### What Lives Where

| Information | Lives In | Reason |
|------------|----------|--------|
| Task sequencing and dependencies | Execution plan | Claude needs to see the full picture, not just one ticket |
| Task status (to do / in progress / done) | Both — Linear is primary, execution plan synced | Linear is where you work; execution plan is where Claude reads |
| Technical context and design rationale | Framework files (technical design, user stories) | Too detailed for ticket descriptions |
| Ticket-level implementation notes | Linear ticket comments | Day-to-day build context that doesn't affect design |
| Design-changing questions from build | Framework questions.md AND Linear ticket (as a reference) | Preserves "single brain" while keeping ticket aware |
| Task-unblocking questions | Linear ticket only | Doesn't need to be in the knowledge base |

---

## Execution Plan Changelog

The bottom of the execution plan file includes a changelog so you can see how the plan evolved:

```markdown
## Changelog

| Date | Change | Triggered By |
|------|--------|-------------|
| 2025-02-10 | Created Linear tickets DC-101 through DC-107 for Data Cleanup epic | User requested ticket creation after story review |
| 2025-02-08 | Moved Integration Setup to Phase 1 (parallel with Cleanup) | Q-INT-003 answered — no dependency on cleanup data |
| 2025-02-05 | Added cross-epic dependency: Data Loads blocked by Cleanup production merge | Design review identified risk of loading into dirty data |
| 2025-01-28 | Initial epic phasing established | Discovery phase analysis |
```

---

## The Morning Briefing — Execution View

When you ask for a briefing focused on execution:

```
EXECUTION STATUS: {Client Name}
==================================

PHASE 1 — In Progress
  Data Cleanup:
    ✅ DC-101: Identify duplicates (Complete)
    🔴 DC-102: Define merge rules (Blocked — Q-DC-001, waiting on client)
    ⬜ DC-103 through DC-107: Not started (waiting on DC-102)

  Legacy Audit:
    🔵 LA-101: Audit existing logic (In Progress)
    ⬜ LA-102 through LA-105: Not started (waiting on audit)

PHASE 2 — Not Started
  Data Loads: Blocked until Cleanup production merge (DC-106) completes
  Integration Setup: Ready to start (moved to Phase 1-parallel)

CRITICAL PATH:
  Q-DC-001 (merge rules question) → DC-102 → DC-103 → DC-104 → DC-105 → DC-106
  This chain is the longest blocker. Everything in Phase 2 waits on it.

RECOMMENDED FOCUS:
  1. Follow up with client on Q-DC-001 — it's on the critical path
  2. Continue LA-101 audit — runs in parallel, no blockers
  3. Start Integration Setup design — newly unblocked
```
