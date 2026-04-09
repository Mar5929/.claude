# Phase {{N}} Tasks: {{Phase Name}}

> Parent Spec: [PHASE_SPEC.md](./PHASE_SPEC.md)
> Total Tasks: {{count}}
> Status: {{0/N complete}}
> Last Updated: {{DATE}}

---

## Execution Order

_Quick reference for task ordering and dependencies within this phase:_

```
Task 1 → Task 2 → Task 3
                → Task 4 (parallel with Task 3)
          Task 5 (after Task 3 + 4)
```

---

## Tasks

### Task 1: {{Task Title}}

| Attribute | Details |
|-----------|---------|
| **Scope** | _Concise description of what this task builds_ |
| **Depends On** | _None / Task numbers within this phase_ |
| **Complexity** | S / M / L |
| **Status** | Not Started / In Progress / Done |
| **Completed** | _Date, if done_ |
| **Linear ID** | _Populated by `/bef sync`, leave blank_ |

**Acceptance Criteria:**
- [ ] {{Specific, testable criterion}}
- [ ] {{Specific, testable criterion}}

**Implementation Notes:**
_Any specific guidance for the AI or developer executing this task:
files to create/modify, patterns to follow, things to watch out for._

---

### Task 2: {{Task Title}}

| Attribute | Details |
|-----------|---------|
| **Scope** | _Concise description_ |
| **Depends On** | Task 1 |
| **Complexity** | S / M / L |
| **Status** | Not Started |
| **Completed** | — |
| **Linear ID** | — |

**Acceptance Criteria:**
- [ ] {{Criterion}}
- [ ] {{Criterion}}

**Implementation Notes:**
_Guidance for execution._

---

_(repeat for each task)_

---

## Summary

| Task | Title | Depends On | Complexity | Status |
|------|-------|-----------|------------|--------|
| 1 | {{title}} | — | {{S/M/L}} | {{status}} |
| 2 | {{title}} | 1 | {{S/M/L}} | {{status}} |
| 3 | {{title}} | 2 | {{S/M/L}} | {{status}} |
| 4 | {{title}} | 2 | {{S/M/L}} | {{status}} |
| 5 | {{title}} | 3, 4 | {{S/M/L}} | {{status}} |
