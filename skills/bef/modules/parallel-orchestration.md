# Parallel Agent Orchestration

Shared pattern for dispatching multiple agents in parallel using worktree isolation,
processing results, merging changes, and cleaning up. Referenced by both
`/bef:execute --agent-teams` (feature tasks) and `/bef:bug-execute` (bug fixes).

## Agent Dispatch

For each work item in the current batch, spawn an agent using the Agent tool with:
- `isolation: "worktree"` — each agent gets its own copy of the repo
- `run_in_background: true` — agents work concurrently

Dispatch ALL agents in the batch simultaneously (single message, multiple Agent tool calls).

Each agent prompt must be self-contained with:
- Full details of the specific work item (task or bug)
- Relevant project context (phase spec, architecture docs, or bug report)
- Clear rules about scope, commit format, and what NOT to modify

## Result Categorization

As each agent completes, categorize the result:

- **Completed**: Agent made changes and committed. Record the commit hash and worktree path.
- **Already Done**: The work was already done (functionality exists, bug already fixed). Mark for status update without code changes.
- **Too Complex**: Agent determined the work exceeds safe scope in isolation (would require architectural changes, touches 10+ files, needs coordination with other work items). Flag for sequential execution or manual review.
- **Failed**: Agent encountered errors or couldn't complete the work. Capture error details for diagnosis.
- **Files Missing**: Referenced files don't exist. Source data may be stale.

Wait for ALL agents in the batch to complete before proceeding.

## Result Report

Present a completion report:

```
## [Batch Label] Results

### Completed (X/Y items)
| Item | Commit | Summary |
|------|--------|---------|
| [ID] | abc1234 | [One-line description of what was done] |

### Already Done (if any)
| Item | Evidence |
|------|----------|
| [ID] | [Why it was already done] |

### Needs Manual Review (if any)
| Item | Reason |
|------|--------|
| [ID] | [Why it exceeded safe scope] |

### Failed (if any)
| Item | Error |
|------|-------|
| [ID] | [Error details] |
```

Offer the user three options:
1. **Review worktrees** — inspect changes in each worktree before merging
2. **Merge all successful** — run the cherry-pick + verify workflow below
3. **Same as last batch** — shorthand for merge all without further prompts
4. **Skip** — abandon this batch's changes (for emergencies)

## Cherry-pick & Verify

When the user approves merging, execute this sequence without pausing between steps:

### 1. Cherry-pick commits

Get commit hashes from each worktree branch (`git log --oneline -1 <branch>`), then
cherry-pick sequentially. Order schema migrations first if any items include database
migrations.

```bash
git cherry-pick <commit-hash>  # repeat for each completed item
```

If there's a merge conflict, pause and ask the user how to resolve it. Otherwise continue.

### 2. Run verification

Run the project's verification commands:

```bash
npx tsc --noEmit     # TypeScript projects
npm test             # or whatever test command exists
```

If verification fails, diagnose and fix the issue before continuing. Common causes:
- Agents used APIs that don't exist in the project's libraries
- Type mismatches from changes that affect shared files
- Import conflicts from multiple agents adding to the same index file

Fix any issues, commit the fix, then re-run verification until clean.

### 3. Clean up worktrees and branches

```bash
git worktree remove .claude/worktrees/agent-XXXX   # for each worktree
git branch -D worktree-agent-XXXX                    # for each branch
```

Note: Use `git branch -D` (force delete) because cherry-picked branches won't show as
"fully merged" — the commits are on main but via cherry-pick, not merge.

### 4. Verify clean state

```bash
git worktree list   # should show only main worktree
git log --oneline -N  # show the new commits (N = number of items)
```

## State Updates

After cherry-pick and verify, the **calling command** is responsible for updating its
own state files. This module does NOT update state — it only handles the shared
dispatch/merge/cleanup pattern. The caller must:

- Update its tracking files (TASKS.md, BUGS.md, bug detail files, etc.)
- Update PROJECT_STATE.md progress counts
- Update external trackers (Linear issues, execution plan completion markers, etc.)

## Important Behaviors

- **Always use worktree isolation.** Never spawn agents on the main working tree.
- **Never auto-merge.** Always present results and wait for user approval.
- **Verify after merge.** Run typecheck/tests to catch integration issues between changes developed in isolation.
- **Respect batch boundaries.** Don't start the next batch until the current one is fully processed.
