---
name: bef
description: >
  Build Execution Framework (BEF) — A deterministic, repeatable framework for decomposing
  full-stack application requirements into executable phases and tasks. Includes an integrated
  bug management subsystem. Triggers on any /bef slash command. Commands: /bef:help, /bef:init,
  /bef:adopt, /bef:prd, /bef:arch, /bef:phases, /bef:deep-dive [phase#], /bef:sync [phase#],
  /bef:execute [phase#] [task#] (supports --agent-teams for wave-based parallel execution),
  /bef:status, /bef:replan, /bef:add-phase, /bef:amend [phase#], /bef:bug-detail [desc],
  /bef:bug-triage [source], /bef:bug-execute [phase], /bef:bug-status. Use this skill for
  any structured project planning, decomposition, execution workflow, or bug management.
  Also triggers when user mentions BEF, build execution framework, project decomposition,
  phase planning, bug triage, bug investigation, or references the /bef command.
---

# Build Execution Framework (BEF)

## Overview

BEF is a deterministic, repeatable framework for taking a software project from idea to
execution through progressive decomposition. Every project follows the same pipeline, produces
the same artifact types in the same structure, and is tracked through a single state file.

The pipeline:
```
PRD → Architecture → Phase Plan → Phase Deep Dive → Linear Sync → Execute
```

Each stage has a clear input, an interaction pattern, and a clear output.

## Core Principles

1. **Always read PROJECT_STATE.md first.** Every command begins by reading the state file
   to understand where the project is. If it doesn't exist, instruct the user to run `/bef:init`.

2. **Never skip stages.** Stages 1-3 are strictly sequential. Stage 4 (deep-dive) requires
   all dependency phases to be deep-dived first. Stage 5 (execute) requires deep-dive + sync.

3. **Files are source of truth.** Planning artifacts in the docs directory are authoritative.
   Linear mirrors execution status but specs live in files.

4. **Interview, don't assume.** At PRD and deep-dive stages, use structured interviews.
   Ask one focused question at a time. Summarize periodically. Never produce the final
   document until the user signals they're done.

5. **Update state after every action.** Every command that changes project state must update
   PROJECT_STATE.md before completing.

6. **Task sizing.** One task = one Claude Code session of focused work. Examples: one database
   migration, one API endpoint with tests, one UI component with integration, one auth flow
   end-to-end. Not "add a field" (too small) and not "build the entire API layer" (too big).

## Directory Structure

Every BEF project uses this exact structure:

```
docs/bef/
  PROJECT_STATE.md          ← Living state tracker (read first, always)
  00-prd/
    PRD.md                  ← Business requirements, user stories, constraints
  01-architecture/
    TECH_STACK.md           ← Languages, frameworks, services, rationale
    DATA_MODEL.md           ← Entities, relationships, schema, constraints
    SYSTEM_ARCHITECTURE.md  ← Component boundaries, APIs, auth, state mgmt
    INTEGRATION_MAP.md      ← External services, APIs, webhooks, failure modes
  02-phase-plan/
    PHASE_PLAN.md           ← Master build order with dependencies
  03-phases/
    phase-01-[name]/
      PHASE_SPEC.md         ← Detailed requirements, approach, edge cases
      TASKS.md              ← Atomic, ordered work items
    phase-02-[name]/
      PHASE_SPEC.md
      TASKS.md
    ...
```

## Template Reference

Templates for each artifact are located in the skill's `templates/` directory. When
creating any artifact, read the corresponding template first and follow its structure exactly.
The templates are:

- `templates/PROJECT_STATE.md` — State tracker
- `templates/PRD.md` — Product Requirements Document
- `templates/TECH_STACK.md` — Technology stack decisions
- `templates/DATA_MODEL.md` — Data model specification
- `templates/SYSTEM_ARCHITECTURE.md` — System architecture
- `templates/INTEGRATION_MAP.md` — External integrations
- `templates/PHASE_PLAN.md` — Phase plan with dependencies
- `templates/PHASE_SPEC.md` — Per-phase detailed specification
- `templates/TASKS.md` — Per-phase task breakdown
- `templates/BUG_REPORT.md` — Individual bug investigation report
- `templates/BUG_EXECUTION_PLAN.md` — Triage execution plan
- `templates/BUG_INDEX.md` — Bug index file (BUGS.md)

## Modules

For detailed specifications that extend the core framework, read the corresponding
module file. Commands reference which modules they require.

- `modules/bug-management.md` — Bug investigation, triage, and execution commands
- `modules/parallel-orchestration.md` — Shared worktree dispatch pattern (used by
  `execute --agent-teams` and `bug-execute`)

---

## Slash Commands

### `/bef:help`

Print the command reference and a brief explanation of the workflow. Use this format:

```
BUILD EXECUTION FRAMEWORK (BEF)
================================

A deterministic framework for decomposing full-stack projects into
executable phases and tasks through progressive refinement.

Pipeline: PRD → Architecture → Phase Plan → Deep Dive → Execute

COMMANDS:
  /bef:help                      Show this guide
  /bef:init                      Bootstrap a new project
  /bef:adopt                     Plug framework into an existing project
  /bef:prd                       Start/resume PRD interview
  /bef:arch                      Generate & review architecture docs
  /bef:phases                    Generate & review phase plan
  /bef:deep-dive [phase#]        Interview-based phase spec + task breakdown
  /bef:sync [phase#]             Push phase tasks to Linear
  /bef:execute [phase#] [task#]  Build a task (add --agent-teams for parallel phase execution)
  /bef:status                    Show current project state
  /bef:replan                    Post-phase checkpoint & adjustment
  /bef:add-phase                 Add a new phase to the plan
  /bef:amend [phase#]            Revise an existing phase spec

BUG MANAGEMENT:
  /bef:bug-detail [desc]         Investigate a bug with Playwright
  /bef:bug-triage [source]       Triage bugs with 3-agent team
  /bef:bug-execute [phase]       Fix bugs in parallel with agent teams
  /bef:bug-status                Show bug tracking summary

RULES:
  • Stages 1-3 (PRD → Arch → Phases) are sequential
  • Deep dives require dependency phases to be deep-dived first
  • Execute requires deep-dive + sync for that phase
  • --agent-teams flag enables wave-based parallel execution with worktree isolation
  • Bug commands require /bef:init but not pipeline stages 1-3
  • Bug subsystem artifacts live in docs/bef/04-bugs/
  • Run replan after completing each phase
  • PROJECT_STATE.md is read before every command
```

---

### `/bef:init`

**Purpose:** Bootstrap a new BEF project.

**Behavior:**
1. Ask for: project name, one-line description.
2. Create the full directory structure under `docs/bef/`, including `04-bugs/` and `04-bugs/reports/`.
3. Read the `templates/PROJECT_STATE.md` template and create the initial `PROJECT_STATE.md`
   with the project name, description, all stages marked as not started, and the Bug Tracking
   section with empty defaults.
4. Confirm to the user that the project is initialized and instruct them to run `/bef:prd` next.

**Output:** Directory structure + `PROJECT_STATE.md`

---

### `/bef:adopt`

**Purpose:** Plug BEF into a project that's already in progress.

**Behavior:**
1. Create the directory structure.
2. Interview the user:
   - "What stage are you at? What's been built so far?"
   - "Do you have any existing planning documents (PRD, architecture notes, specs)?"
   - "How many phases would you estimate the project has, and which are done?"
3. For existing docs: ask the user to point you at the files. Read them and migrate/adapt
   them into the standard BEF artifact format in the correct directories.
4. For completed phases: mark them as done in PROJECT_STATE.md. Create a brief retroactive
   summary in a PHASE_SPEC.md so later phases have context. Do NOT require the user to
   re-spec completed work.
5. For the current in-progress phase: create the PHASE_SPEC.md and TASKS.md, then ask the
   user which tasks are already done. Mark them accordingly.
6. For future phases: leave them as not started.
7. Ask: "Do you have any existing bug tracking? Bug reports, issue lists, or a bug index?"
   If yes, migrate those artifacts into `docs/bef/04-bugs/` and populate the Bug Tracking section.
8. Update PROJECT_STATE.md to reflect the true current state.

**Output:** Full directory structure + migrated artifacts + accurate PROJECT_STATE.md

---

### `/bef:prd`

**Purpose:** Create the Product Requirements Document through structured interview.

**Gate:** `/bef:init` or `/bef:adopt` must have been run.

**Interview Protocol:**

Conduct the interview one topic at a time. For each topic, ask focused questions, listen,
and summarize what you've heard before moving on. The topics are:

1. **Business Context** — What problem does this solve? Who is it for? What's the business
   model or motivation?
2. **User Personas** — Who are the distinct types of users? What are their goals and pain points?
3. **Core Features** — What are the must-have features for launch? Walk through each one.
4. **User Flows** — For each core feature, what's the step-by-step user journey?
5. **Non-Functional Requirements** — Performance targets, security requirements, scalability
   needs, compliance/regulatory constraints.
6. **Scope Boundaries** — What's explicitly OUT of scope? What's deferred to later phases?
7. **Known Constraints** — Timeline, budget, team size, existing systems to integrate with,
   technical constraints.
8. **Success Criteria** — How do we know this project succeeded? What metrics matter?

After completing all topics, ask: "Is there anything else I should know that we haven't
covered?"

When the user signals they're done, produce the final PRD.md using the template. Present
it for review. Iterate if needed. When approved, mark Stage 1 complete in PROJECT_STATE.md.

**Output:** `docs/bef/00-prd/PRD.md`

---

### `/bef:arch`

**Purpose:** Generate the four architecture documents based on the PRD.

**Gate:** Stage 1 (PRD) must be complete.

**Behavior:**
1. Read the PRD.md.
2. Generate drafts of all four architecture documents using the templates:
   - TECH_STACK.md
   - DATA_MODEL.md
   - SYSTEM_ARCHITECTURE.md
   - INTEGRATION_MAP.md
3. Present each document one at a time for the user's review.
4. For each document, ask: "Does this look right? Anything to change or add?"
5. Iterate until the user approves each one.
6. When all four are approved, mark Stage 2 complete in PROJECT_STATE.md.

**Important:** For the DATA_MODEL.md, be thorough about relationships, indexes, and
constraints. This document directly informs the schema migration phase. For TECH_STACK.md,
include specific version numbers and rationale for each choice.

**Output:** Four files in `docs/bef/01-architecture/`

---

### `/bef:phases`

**Purpose:** Generate the master Phase Plan.

**Gate:** Stage 2 (Architecture) must be complete.

**Behavior:**
1. Read the PRD + all four architecture documents.
2. Propose the full Phase Plan using the template. For each phase, include:
   - Phase number and name
   - One-paragraph scope description
   - Dependencies (which phases must complete first)
   - What it unlocks (which phases can start after this)
   - Complexity estimate (S/M/L/XL)
   - Whether it can run in parallel with other phases
3. Present the plan for review. Discuss ordering, merging/splitting phases, adjusting scope.
4. When approved, mark Stage 3 complete in PROJECT_STATE.md. Update the Phase Overview table.

**Guidelines for phase decomposition:**
- Database/schema work is almost always Phase 1
- Auth/identity is typically Phase 2 (depends on schema)
- Core business logic before UI
- Integrations and advanced features toward the end
- Each phase should be testable independently when its dependencies are met
- Aim for 4-8 phases for a typical full-stack app. More than 10 suggests phases are too granular.

**Output:** `docs/bef/02-phase-plan/PHASE_PLAN.md` + updated PROJECT_STATE.md

---

### `/bef:deep-dive [phase-number]`

**Purpose:** Create the detailed Phase Spec and Task Breakdown for a specific phase.

**Gate:** Stage 3 (Phase Plan) must be complete. All dependency phases for the target phase
must already have their deep-dives complete.

**Behavior:**
1. Read: PROJECT_STATE.md, PHASE_PLAN.md, all architecture docs, and any completed
   PHASE_SPEC.md files for dependency phases.
2. Announce: "Starting deep dive for Phase [N]: [Name]. I'm going to interview you on
   five areas."
3. Interview one area at a time:

   **a. Functional Requirements** — "Let's get specific about what this phase builds.
   Walk me through each piece of functionality." Probe for details, confirm understanding.

   **b. Technical Approach** — "Here's how I'd approach building this technically. Does
   this align with your thinking?" Propose an approach, discuss alternatives.

   **c. Edge Cases & Error Handling** — "Here are the edge cases I see. What am I missing?"
   List the ones you can identify, ask for others. Cover: invalid inputs, race conditions,
   failure modes, empty states, permission boundaries.

   **d. Integration Points** — "This phase touches [X, Y, Z] from prior phases. Let me
   confirm the contracts." Verify API contracts, data shapes, auth flows.

   **e. Acceptance Criteria** — "How do we know this phase is done? Here's what I'd test."
   Propose criteria, refine together.

4. After the interview, produce:
   - PHASE_SPEC.md using the template (detailed spec for this phase)
   - TASKS.md using the template (atomic, ordered work items)
5. Present both for review. Iterate until approved.
6. Create the phase directory under `03-phases/` and save both files.
7. Update PROJECT_STATE.md: mark the phase's Spec and Tasks columns as complete.

**Output:** `docs/bef/03-phases/phase-[NN]-[name]/PHASE_SPEC.md` + `TASKS.md`

---

### `/bef:sync [phase-number]`

**Purpose:** Push tasks from a phase's TASKS.md to Linear as a milestone with linked issues.

**Gate:** The phase must have a completed TASKS.md.

**Behavior:**
1. Read the TASKS.md for the specified phase.
2. Determine the appropriate Linear team and project. Ask the user if unclear.
3. Create a milestone in Linear for this phase:
   - Name: "Phase [N]: [Phase Name]"
   - Description: One-paragraph scope from PHASE_PLAN.md
4. For each task in TASKS.md, create a Linear issue:
   - Title: Task title from TASKS.md
   - Description: Task scope + acceptance criteria from TASKS.md
   - Associate with the milestone
   - Set dependency relationships (blocked by / blocking) based on the dependency
     links in TASKS.md
   - Label with phase number
5. Record the Linear issue IDs back into TASKS.md in a `linear_id` field for each task,
   so there's a two-way reference.
6. Update PROJECT_STATE.md: mark the phase's Linear column as complete.

**Linear tool usage:** Use the Linear MCP tools to create milestones and issues. Specifically:
- Use `list_teams` to identify the correct team if not already known.
- Use `list_projects` to find the correct project.
- Use `save_milestone` to create the phase milestone.
- Use `save_issue` to create each task as an issue.
- Link issues using blocked-by/blocking relationships.

**Output:** Linear milestone + issues created, TASKS.md updated with Linear IDs.

---

### `/bef:execute [phase-number] [task-number]`

**Purpose:** Execute a specific task — write the actual code.

**Gate:** The phase must be deep-dived and synced. The task's dependency tasks must be complete.

**Behavior:**
1. Read: PROJECT_STATE.md to confirm gates are met.
2. Read: PHASE_SPEC.md for the full phase context.
3. Read: The specific task from TASKS.md (scope, acceptance criteria, dependencies).
4. Read: Any relevant architecture docs (DATA_MODEL.md for schema work, SYSTEM_ARCHITECTURE.md
   for API work, etc.).
5. Announce what you're about to build and confirm with the user.
6. Build the task. Follow the acceptance criteria exactly.
7. When complete, update:
   - TASKS.md: mark the task as done with completion date.
   - PROJECT_STATE.md: update the execution progress (e.g., "3/8 tasks").
   - Update the Linear issue status if the Linear MCP tools are available.
8. Announce completion and what the next task is.

**Output:** Working code + updated state files.

#### Phase-Level Execution (Agent Teams)

**Trigger:** `/bef:execute [phase#] --agent-teams`

When the `--agent-teams` flag is present, the execute command enters orchestration mode.
Instead of executing a single task, it analyzes all remaining tasks in the phase, groups
them into parallelizable waves, and dispatches agent teams to execute tasks simultaneously
using worktree isolation.

The orchestrator (this session) manages the full lifecycle:
analysis → dispatch → merge → verify → state update.

**Critical rule:** Worker agents only write code and commit. They never touch TASKS.md,
PROJECT_STATE.md, or any `docs/bef/` tracking files. Only the orchestrator updates state.

**Step 1: Analyze Ready Tasks**

1. Read TASKS.md for the target phase.
2. Identify all tasks where:
   - Status = "Not Started"
   - All tasks listed in "Depends On" have Status = "Done"
3. These are the "ready tasks" — their dependencies are satisfied.
4. If 0 tasks are ready: report that all tasks are either done or blocked, and which
   tasks are blocking progress.
5. If 1 task is ready: skip agent teams, execute it directly using single-task mode above.
6. If 2+ tasks are ready: proceed to Step 2.

**Step 2: Detect File Overlaps & Form Waves**

For each ready task, extract the files it will likely touch from:
- The "Implementation Notes" field in TASKS.md (files to create/modify)
- The task scope description (infer which modules/directories are affected)
- The PHASE_SPEC.md file/module structure section

Build a conflict graph: two tasks conflict if they are likely to modify the same file(s).

Form parallel groups (waves):
- Tasks with NO file overlap with each other → same wave (can run in parallel)
- Tasks that overlap with another ready task → defer to a later wave
- Prefer larger waves (maximize parallelism) while keeping conflicting tasks separate

If ALL ready tasks conflict with each other: recommend sequential execution instead
of agent teams. Ask the user if they want to proceed sequentially or override.

**Step 3: Present Execution Plan**

Display the wave plan for user approval:

```
## Phase [N] Agent Teams Execution Plan

**Ready tasks (dependencies met):** [count]

### Wave 1
| Task | Title | Complexity | Touches |
|------|-------|-----------|---------|
| 3 | [title] | S | src/api/users.ts, prisma/migrations/ |
| 5 | [title] | M | src/components/dashboard/ |

**File overlap check:** No conflicts detected ✓

### Wave 2 (after Wave 1 merges)
| Task | Title | Complexity | Why deferred |
|------|-------|-----------|-------------|
| 4 | [title] | M | Shares src/api/routes.ts with Task 3 |
| 6 | [title] | S | Depends on Task 3 (newly unblocked) |

### Still Blocked
| Task | Waiting On |
|------|-----------|
| 8 | Tasks 6, 7 |

**Estimated waves:** 2 parallel + 0 sequential
**Proceed with Wave 1?** (Y / modify / switch to sequential)
```

Wait for user confirmation before dispatching.

**Step 4: Dispatch Agent Team for Current Wave**

For each task in the current wave, spawn an agent using the Agent tool with
`isolation: "worktree"` and `run_in_background: true`.

Agent prompt template:

```
You are executing a specific task from a BEF phase plan. Build exactly what the
task specifies, following the acceptance criteria precisely.

## Task Details
[Full task block from TASKS.md — scope, acceptance criteria, implementation notes]

## Phase Context
[PHASE_SPEC.md contents]

## Architecture Context
[Only the relevant architecture docs — DATA_MODEL.md for schema work,
SYSTEM_ARCHITECTURE.md for API work, INTEGRATION_MAP.md for external services, etc.
Do NOT include all docs — select based on the task's domain.]

## Rules
1. Build ONLY what this task specifies. Do not work on other tasks.
2. Follow the acceptance criteria exactly — each criterion must be met.
3. Do NOT modify TASKS.md, PROJECT_STATE.md, or any docs/bef/ tracking files.
4. Commit your work with message: `feat(phase-N/task-M): <short description>`
5. If the task is already done (functionality exists), report this — don't duplicate.
6. If the task is more complex than expected (would require changes beyond task scope),
   stop and report this rather than attempting a risky implementation.
7. Run relevant tests/typechecks after implementation to verify changes work.
```

Dispatch ALL agents in the wave simultaneously (single message, multiple Agent tool calls).

**Step 5: Process Wave Results**

As each agent completes, categorize the result:

- **Completed**: Agent built the task and committed. Record commit hash + worktree path.
- **Already Done**: Functionality already exists. Mark for status update without code changes.
- **Too Complex**: Agent determined task exceeds safe scope in isolation. Flag for
  sequential execution in a later wave.
- **Failed**: Agent encountered errors. Capture error details for diagnosis.

Wait for ALL agents in the wave to complete, then present a wave report:

```
## Wave [W] Results

### Completed (X/Y tasks)
| Task | Commit | Summary |
|------|--------|---------|
| 3 | abc1234 | Built user API endpoint with validation |
| 5 | def5678 | Dashboard component with data fetching |

### Needs Manual Execution (if any)
| Task | Reason |
|------|--------|
| 7 | Too complex — requires auth middleware refactor |

### Failed (if any)
| Task | Error |
|------|-------|
| 9 | TypeScript compilation failed |
```

Offer the user four options:
1. **Review worktrees** — inspect changes before merging
2. **Merge all successful** — run the cherry-pick + verify workflow
3. **Same as last wave** — shorthand for merge all without further prompts
4. **Skip to next wave** — abandon this wave's changes (for emergencies)

**Step 6: Cherry-pick, Verify, and Update State**

When the user approves merging:

1. **Cherry-pick** each completed agent's commit(s) onto the working branch, sequentially.
   If a cherry-pick conflict occurs, pause and ask the user to resolve it.

2. **Verify** — run the project's test/typecheck commands. If verification fails, diagnose
   and fix integration issues between changes that were developed in isolation. Common
   causes: shared imports modified by multiple tasks, index files both tasks updated.

3. **Update TASKS.md** — for each completed task:
   - Set Status to "Done"
   - Set Completed to today's date
   - Check off acceptance criteria

4. **Update PROJECT_STATE.md** — update the execution progress count (e.g., "5/8 done").

5. **Update Linear** — if Linear MCP tools are available, update issue statuses.

6. **Clean up** — remove worktrees and branches for completed agents.

**Step 7: Advance to Next Wave**

After the current wave is merged and verified:

1. Re-read TASKS.md to get the updated state.
2. Re-analyze: identify newly ready tasks (dependencies just satisfied by this wave).
3. If new ready tasks exist:
   - Re-run file overlap detection and form the next wave.
   - Present the next wave plan for user approval.
   - Repeat from Step 4.
4. If no ready tasks but incomplete tasks remain: report which tasks are blocked and why.
5. If all tasks are Done: announce phase completion. Suggest running `/bef:replan`.

This creates a natural "wave" execution pattern:

```
Wave 1: Tasks 3, 5 (parallel) → merge → verify
Wave 2: Tasks 4, 6, 7 (parallel, newly unblocked) → merge → verify
Wave 3: Task 8 (single, execute directly) → verify
Phase complete ✓
```

---

### `/bef:status`

**Purpose:** Print the current project state.

**Behavior:**
1. Read PROJECT_STATE.md.
2. Print a clean summary of: pipeline progress, phase overview table, current focus,
   and any recent replan notes.
3. If there are incomplete deep-dives blocking the next execution, call it out.
4. If `docs/bef/04-bugs/BUGS.md` exists, include the Bug Tracking summary
   (open/fixed counts by severity, active bug phase).

**Output:** Formatted state summary (not a file, just console output).

---

### `/bef:replan`

**Purpose:** Post-phase checkpoint. Review what was built against the plan and adjust.

**Behavior:**
1. Read: PROJECT_STATE.md, PHASE_PLAN.md, PRD.md, and all completed PHASE_SPEC.md files.
2. Assess:
   - Did the completed phase deliver everything in its spec?
   - Are there any gaps between what's been built and what the PRD requires?
   - Do any future phase specs or the phase plan need adjustment based on what was learned?
   - Are there open bugs in `docs/bef/04-bugs/BUGS.md`? Do any block future phases?
   - Should a bug-fix wave be inserted before the next phase execution?
   - Did bug fixes during execution change assumptions that affect upcoming phase specs?
3. Report findings to the user. Propose changes if needed.
4. If changes are needed:
   - Update PHASE_PLAN.md with revised phases, ordering, or scope.
   - If a new phase is needed, create it with a new phase number.
   - If tasks need to be added to an existing future phase, note them for the next deep-dive.
5. Log the replan in the Replan Log section of PROJECT_STATE.md with date and summary.

**After final phase completion:** Run a full PRD audit. Compare everything built across all
phases against every requirement in the PRD. Flag gaps, drift, and any missing functionality.
Propose remediation phases if needed.

**Output:** Updated PHASE_PLAN.md + PROJECT_STATE.md with replan log entry.

---

### `/bef:add-phase`

**Purpose:** Add a new phase to the project (post-initial-planning or during replan).

**Behavior:**
1. Read: PROJECT_STATE.md, PHASE_PLAN.md, all completed phase specs.
2. Interview: "What does this new phase need to accomplish? What does it depend on?"
3. Assign the next sequential phase number (even if it logically goes between earlier phases —
   ordering is handled by the dependency graph, not numbering).
4. Add the phase to PHASE_PLAN.md with full details (scope, dependencies, unlocks, complexity).
5. Update PROJECT_STATE.md phase overview table.
6. Inform the user to run `/bef:deep-dive [N]` when ready to spec it out.

**Output:** Updated PHASE_PLAN.md + PROJECT_STATE.md

---

### `/bef:amend [phase-number]`

**Purpose:** Revise an existing phase spec due to scope changes or new requirements.

**Behavior:**
1. Read: The current PHASE_SPEC.md and TASKS.md for the target phase.
2. Interview: "What's changing? New requirements, removed requirements, or modified approach?"
3. Update the PHASE_SPEC.md with the changes. Clearly mark what changed and why (add a
   "Revision History" section at the bottom).
4. Update TASKS.md: add new tasks, remove obsolete ones, adjust existing ones.
5. If the phase has already been synced to Linear, inform the user that they should re-run
   `/bef:sync [phase#]` to update Linear.
6. Update PROJECT_STATE.md.

**Output:** Updated PHASE_SPEC.md + TASKS.md + PROJECT_STATE.md

---

## State Management Rules

1. **PROJECT_STATE.md is the single source of truth for project progress.** Every command
   reads it first and updates it when done.

2. **State transitions are explicit.** A stage is only marked complete when the user has
   reviewed and approved the output.

3. **Idempotent operations.** Running a command twice doesn't create duplicates. If a sync
   has already been run, re-running it updates existing Linear issues rather than creating new ones.

4. **Replan log is append-only.** Never delete replan log entries. They form the project's
   decision history.

5. **Phase numbering is sequential and permanent.** Phase 3 is always Phase 3, even if its
   scope changes. New phases get the next available number. This prevents confusion from
   renumbering.

## Interview Protocol Rules

For `/bef:prd` and `/bef:deep-dive`:

1. Ask **one focused question** at a time. Do not dump a list of 10 questions.
2. **Summarize** what you've heard after every 2-3 exchanges. "Here's what I have so far..."
3. **Probe** when answers are vague. "Can you give me a specific example?" or "What happens
   when [edge case]?"
4. **Explicitly transition** between topics. "Great, I think I have a solid picture of the
   user flows. Let's move on to non-functional requirements."
5. **Ask for closure** before producing the document. "Is there anything else on this topic
   before I write it up?"
6. **Never assume.** If something is ambiguous, ask. Don't fill in gaps with guesses.
