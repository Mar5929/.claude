# Bug Execution Plan

Generated: {{DATE}}
Source: {{bug source path/URL}}
Total Bugs: {{count}}
Phases: {{count}}

---

## Severity Reclassifications

_Only bugs where final severity differs from original._

| Bug ID | Original | Final | Reason |
|--------|----------|-------|--------|
| — | — | — | — |

---

## Phase 1: {{Goal Title}}

**Goal:** {{what this phase accomplishes}}
**Depends on:** None
**Complexity:** {{Low / Medium / High}}

**Bugs (parallel-safe):**

| Bug ID | Title | Final Severity | Fix Complexity | Notes |
|--------|-------|---------------|---------------|-------|
| — | — | — | — | — |

**Risks:** {{what could go wrong}}
**Verification:** {{how to confirm success}}
**Estimated scope:** {{# files, rough size of changes}}

---

## Phase 2: {{Goal Title}}

**Goal:** {{what this phase accomplishes}}
**Depends on:** Phase 1
**Complexity:** {{Low / Medium / High}}

**Bugs (parallel-safe):**

| Bug ID | Title | Final Severity | Fix Complexity | Notes |
|--------|-------|---------------|---------------|-------|
| — | — | — | — | — |

**Risks:** {{what could go wrong}}
**Verification:** {{how to confirm success}}
**Estimated scope:** {{# files, rough size of changes}}

---

_(repeat for each phase)_

---

## Dependency Graph

```
Phase 1 (foundation) → Phase 2 → Phase 3
                     → Phase 4 (parallel with Phase 3)
```

## Summary

- **Phase 1 (parallel):** {{BUG-IDs}}
- **Phase 2 (parallel, after Phase 1):** {{BUG-IDs}}

Total phases: {{count}}
Estimated parallel developer slots needed: {{max bugs in any single phase}}
