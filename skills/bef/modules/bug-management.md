# BEF Bug Management Subsystem

Bug investigation, triage, and execution commands integrated into the BEF framework.
Bugs are a parallel track — they can be filed at any point during the BEF pipeline and
do not require pipeline stages 1-3 (PRD, Architecture, Phase Plan) to be complete.

All bug artifacts live in `docs/bef/04-bugs/`. State is tracked in the Bug Tracking
section of `docs/bef/PROJECT_STATE.md`.

## Directory Structure

```
docs/bef/04-bugs/
  BUGS.md                     ← Index file (severity counts, status table)
  BUG-EXECUTION-PLAN.md       ← Triage output (phased fix plan)
  reports/
    BUG-001.md                ← Individual investigation reports
    BUG-002.md
    ...
```

## Template Reference

Templates for bug artifacts are in the skill's `templates/` directory:
- `templates/BUG_REPORT.md` — Individual bug investigation report
- `templates/BUG_EXECUTION_PLAN.md` — Triage execution plan
- `templates/BUG_INDEX.md` — Bug index file (BUGS.md)

---

## `/bef:bug-detail [description]`

**Purpose:** Investigate a bug using Playwright browser automation. Produces a detailed
bug report with console errors, network failures, DOM state, and root cause analysis.

**Gate:** PROJECT_STATE.md must exist (`/bef:init` or `/bef:adopt` has been run).
No pipeline stage gates — bugs can be filed at any point.

### Configuration

Check PROJECT_STATE.md for a `### Bug Detailer Config` table in the Bug Tracking section.

**First run — setup interview.** If no config exists, ask one question at a time:

1. **"How do I start the dev server?"** — e.g., `npm run dev`. Ask what port (default: 3000).
2. **"What's the app URL?"** — e.g., `http://localhost:3000`
3. **"Does the app require login?"** — If yes: auth system type, test credentials.
   Store credentials in `.env.local` (gitignored). Suggest `TEST_USER_EMAIL` and
   `TEST_USER_PASSWORD` as variable names. Ask user for the values.
4. **"Any context files for understanding expected behavior?"** — PRD, tech spec, etc.
   If BEF architecture docs exist, suggest those automatically.

Save config to PROJECT_STATE.md's Bug Detailer Config table.

**Subsequent runs.** Read config from PROJECT_STATE.md. Don't re-ask. Confirm briefly:
"Using saved config — app at localhost:3000, bugs go to docs/bef/04-bugs/reports/. What's the bug?"

**Backward compatibility.** If PROJECT_STATE.md has no config but CLAUDE.md has a
`## Bug Detailer Config` section, offer to migrate: "Found bug config in CLAUDE.md.
Want me to migrate it to PROJECT_STATE.md?" If yes, copy values and remove the
CLAUDE.md section.

### Bug Investigation

1. **Get the bug description.** Ask: "What's going on?" One clarifying question max
   if the description is too vague to know where to navigate.

2. **Start the dev server.** Check if running (`lsof -i :<port> -t`). If not, start
   in background and poll for readiness (up to 30 seconds).

3. **Navigate and authenticate.** Open app URL with `browser_navigate`. If auth required,
   navigate to login URL, read credentials from `.env.local`, fill form, wait for redirect.

4. **Reproduce the bug.** Navigate to the relevant area. Take snapshots before and after
   interactions. Follow the user's described steps (or infer reasonable ones).

5. **Gather evidence.** Collect ALL of:
   - **Console errors/warnings** — `browser_console_messages`
   - **Network failures** — `browser_network_requests` (4xx, 5xx, timeouts, CORS)
   - **DOM state** — `browser_snapshot` (missing elements, wrong text, hidden elements)
   - **Screenshots** — `browser_take_screenshot` (buggy state, error modals)
   - **Visual anomalies** — broken layouts, overlapping elements, missing styles

6. **Dig deeper.** Based on evidence:
   - Console error → identify source file and line from stack trace
   - API failure → check response body for error details
   - Missing DOM elements → check if parent component rendered
   - Navigate related pages to determine bug scope

7. **Analyze and classify.** Read relevant source code (follow file paths from evidence).
   Determine root cause, affected files, and severity:
   - **CRITICAL:** Feature completely broken, users blocked, data loss, security issue
   - **HIGH:** Feature partially broken, painful workaround, primary workflow affected
   - **MEDIUM:** Feature works incorrectly or poor UX, secondary workflow
   - **LOW:** Cosmetic, minor inconvenience, edge case

8. **Write the bug report.** Read the `templates/BUG_REPORT.md` template. Create the
   report at `docs/bef/04-bugs/reports/BUG-XXX.md` using the next bug ID from
   PROJECT_STATE.md.

9. **Update tracking.**
   - Update `docs/bef/04-bugs/BUGS.md`: add row, update severity counts
   - Update PROJECT_STATE.md: increment Next Bug ID, update Open count
   - If the bug clearly relates to a BEF phase, note it in the Phase field

10. **Present summary.** Show: bug ID, title, severity, key evidence, root cause summary,
    where the report was saved. Ask: "Want me to investigate another bug?"

### Multiple Bugs Per Session

Keep browser open, keep dev server running, increment bug ID for each. Don't re-ask config.

### Edge Cases

- **App won't start:** Report startup error, suggest fix if obvious
- **Login fails:** Report auth failure, check `.env.local` credentials
- **Bug can't be reproduced:** Create report noting "Could not reproduce" with evidence gathered
- **Multiple bugs discovered:** Stay focused on the reported bug, mention others at the end
- **Close browser** when all investigations for the session are complete

---

## `/bef:bug-triage [source]`

**Purpose:** Triage and prioritize bugs using a 3-agent team (Business Analyst, Technical
Lead, Triage Coordinator). Produces a phased execution plan.

**Gate:** PROJECT_STATE.md must exist. At least one bug report in `docs/bef/04-bugs/reports/`
or user provides an external source.

### Gather Input

**Bug source.** Ask: "Where are the bugs?" Detect source type:
- **Default (no argument):** Read from `docs/bef/04-bugs/reports/` and `docs/bef/04-bugs/BUGS.md`
- **Local folder** (e.g., `.planning/bugs/`): Read all `.md` files
- **Local file** (e.g., `BUGS.md`, `issues.csv`): Parse bug entries
- **GitHub Issues:** Use `gh issue list` and `gh issue view`. Accept `owner/repo`, GitHub URL,
  or "this repo's GitHub issues" (auto-detect from git remote)

**Context files.** Ask: "Any additional context?" Then automatically offer BEF's own docs:
"I also found these BEF project documents that could help the triage agents:
- `docs/bef/00-prd/PRD.md`
- `docs/bef/01-architecture/SYSTEM_ARCHITECTURE.md`
- `docs/bef/01-architecture/DATA_MODEL.md`
Want me to include them as context?"

This automatic context from BEF's architecture docs is a key integration advantage over
the standalone skill.

### Spawn the Agent Team

#### Agent 1: Business Analyst (parallel)

Evaluates bugs from the USER's perspective:
- User Impact rating (Critical/High/Medium/Low)
- Affected Workflows (specific user flows disrupted)
- User-Facing Severity Justification
- Reclassification Recommendation (if current severity is wrong)

Output: per-bug assessment + "Top 5 Most Impactful Bugs" summary.

Prompt must include: full bug details, context files (PRD, BEF architecture docs if included).

#### Agent 2: Technical Lead (parallel)

Evaluates bugs from a CODE perspective:
- Fix Complexity (Trivial/Simple/Moderate/Complex)
- Blast Radius (files/systems affected)
- Dependencies (which bugs block/enable others: "Must fix BUG-001 before BUG-004 because...")
- Conflict Risk (bugs that CANNOT be worked on simultaneously)
- Technical Severity Assessment
- Reclassification Recommendation

Output: per-bug assessment + "Dependency Graph Summary" showing critical path.

Prompt must include: full bug details, context files (tech spec, architecture docs if included).

#### Agent 3: Triage Coordinator (AFTER Agents 1 and 2 complete)

**Do NOT spawn in parallel with BA and TL.** Wait for both to complete, then spawn with
their outputs.

Synthesizes BA + TL reports into an actionable plan:
- **Final Severity Classification** — weighs both user impact and technical risk.
  When overriding BA or TL, explains reasoning.
- **Phased Execution Plan** — groups bugs into numbered phases:
  - Phase 1: no dependencies, unblocks the most work (foundation fixes)
  - Phase 2: depends on Phase 1
  - Phase 3+: continues the pattern
  - Within each phase: all bugs can be worked in parallel (no conflicts, no dependencies)
  - Max 5 bugs per phase
  - Conflicting bugs (TL flagged) MUST be in different phases
- **Execution Notes per phase:** Goal, Complexity, Risks, Verification

### Write the Execution Plan

1. Write plan to `docs/bef/04-bugs/BUG-EXECUTION-PLAN.md` using the template.
2. Update PROJECT_STATE.md Bug Tracking section: set Active Bug Phase, update counts.
3. Present summary: total bugs, phases, reclassifications, Phase 1 bugs.
4. Tell user: "Use `/bef:bug-execute [phase]` to start fixing bugs."

### Important Behaviors

- **Never modify bug files.** Triage is read-only analysis.
- **Respect existing severity.** Agents acknowledge it; only recommend reclassifications.
- **Be transparent about disagreements.** Show Coordinator's reasoning when BA and TL disagree.
- **Handle sparse data gracefully.** Note "insufficient detail" rather than guessing.
- **Scale to any size.** For 30+ bugs, mention it will take a few minutes.

---

## `/bef:bug-execute [phase]`

**Purpose:** Execute bug fixes by spawning parallel developer agents in worktrees.

**Gate:** `docs/bef/04-bugs/BUG-EXECUTION-PLAN.md` must exist (run `/bef:bug-triage` first).

### Find and Parse the Execution Plan

1. Read `docs/bef/04-bugs/BUG-EXECUTION-PLAN.md`.
2. Parse: total phases, which are completed (check for `[COMPLETED]` markers), next incomplete.

### Phase Selection

- If user specified a phase number (e.g., `/bef:bug-execute 2`): use that phase.
- If user said "next": use next incomplete phase.
- If no argument: show status of all phases and ask:

```
## Bug Execution Plan Status

Phase 1: Foundation Fixes [COMPLETED]
  - BUG-001, BUG-003, BUG-002 (3 bugs)

Phase 2: UI and Navigation [READY]
  - BUG-004, BUG-007, BUG-008 (3 bugs)

Phase 3: Data Integrity [BLOCKED by Phase 2]
  - BUG-006, BUG-014 (2 bugs)

Which phase? (or "next" for Phase 2)
```

If selected phase depends on an incomplete prior phase, warn and ask for confirmation.

### Spawn Developer Agents

For each bug in the selected phase, spawn a developer agent using the shared
Parallel Agent Orchestration pattern (read `modules/parallel-orchestration.md`).

**Developer agent prompt template:**

```
You are a developer fixing a specific bug. Your job is to understand the bug,
implement the fix, and verify it works.

## Bug Details
[Full contents of docs/bef/04-bugs/reports/BUG-XXX.md]

## Application Context
[Tech stack, relevant architecture docs from docs/bef/01-architecture/]

## Your Task

1. **Understand the bug**: Read files in the bug's "Location" and "Related Files".
   Understand the root cause before writing code.

2. **Implement the fix**: Follow the "Fix" section if one exists. Otherwise determine
   the approach from root cause. Minimal change — don't refactor or add features.

3. **Verify**: Run tests/typecheck. Note unrelated failures but don't fix them.

4. **Commit**: Single atomic commit: `fix(BUG-XXX): <short description>`

## Rules
- Only modify files related to this bug.
- If referenced files don't exist, note it — codebase may have changed.
- If bug is already fixed, report this instead of making changes.
- If fix is more complex than expected (architectural changes, 10+ files), stop and report.
- Do NOT update BUGS.md, BUG-XXX.md reports, or PROJECT_STATE.md.
```

Dispatch ALL bugs in the phase simultaneously per the orchestration module.

### Process Results and Merge

Follow the Parallel Agent Orchestration pattern for:
- Result categorization
- Phase completion report
- User approval
- Cherry-pick, verify, clean up

### State Updates (after successful merge)

1. **Bug detail files:** Change `**Status:** Open` to `**Status:** Fixed` in each
   fixed bug's report file (`docs/bef/04-bugs/reports/BUG-XXX.md`).
2. **BUGS.md:** Update status column to "Fixed", recalculate severity summary counts.
3. **Execution plan:** Add completion marker: `## Phase N: Title [COMPLETED - YYYY-MM-DD]`
4. **PROJECT_STATE.md:** Update Bug Tracking section (Fixed count, Open count,
   Active Bug Phase to next phase or "None").
5. **Linear:** If Linear MCP tools available, update issue statuses.

### Next Phase

After completion, announce:
"Phase [N] complete. [X] bugs fixed, [Y] need review. Phase [N+1] is now unblocked:
[title] ([Z] bugs). Run `/bef:bug-execute next` to continue."

If all phases complete:
"All bug phases complete! [total] bugs fixed across [phases] phases."

### Edge Cases

- **Single bug in a phase:** Still use worktree isolation, no parallelization needed.
- **Empty phase** (all bugs already fixed): Mark complete, move to next.
- **Bug ID in plan doesn't match any report:** Skip and flag in report.
- **User wants to skip a bug:** Don't spawn agent, mark as "Skipped" in report.
- **Retry a failed bug:** Support `/bef:bug-execute <phase> --retry BUG-XXX`

---

## `/bef:bug-status`

**Purpose:** Display bug tracking summary.

**Behavior:**
1. Read PROJECT_STATE.md Bug Tracking section.
2. Read `docs/bef/04-bugs/BUGS.md` if it exists.
3. Print a clean summary:

```
## Bug Tracking Summary

**Open:** X (Critical: A, High: B, Medium: C, Low: D)
**Fixed:** Y
**Total:** Z

**Active Bug Phase:** Phase 2: UI and Navigation (3 bugs, 1/3 fixed)
**Execution Plan:** docs/bef/04-bugs/BUG-EXECUTION-PLAN.md

**Bugs Blocking BEF Phases:**
- BUG-005 (HIGH) blocks Phase 4 execution — affects auth middleware
```

4. If there are critical/high open bugs, call them out.
5. If any bugs reference a BEF phase in their Phase field, note which phases are affected.
