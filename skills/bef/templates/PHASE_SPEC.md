# Phase {{N}} Spec: {{Phase Name}}

> Parent: [Phase Plan](../../02-phase-plan/PHASE_PLAN.md)
> Depends On: {{Phase dependencies or "None"}}
> Status: Draft | Approved
> Last Updated: {{DATE}}

---

## 1. Scope Summary

_One paragraph from the Phase Plan, expanded with any additional context from the deep dive._

---

## 2. Functional Requirements

### 2.1 {{Requirement Group Name}}

_Detailed description of this functional area:_

- **What it does:** _Specific behavior_
- **Inputs:** _What data or actions trigger this_
- **Outputs:** _What the system produces or changes_
- **Business rules:** _Any logic or constraints_

### 2.2 {{Requirement Group Name}}

_(repeat for each functional area in this phase)_

---

## 3. Technical Approach

### 3.1 Implementation Strategy

_How will this phase be built? Describe the approach at a technical level:
patterns, architecture decisions specific to this phase, key libraries or tools._

### 3.2 File/Module Structure

_What new files, modules, or directories will this phase create?_

```
src/
  {{module}}/
    {{file}}.{{ext}} — {{purpose}}
    {{file}}.{{ext}} — {{purpose}}
```

### 3.3 API Contracts (if applicable)

_New or modified endpoints:_

#### `{{METHOD}} {{/path}}`
- **Request:** `{{shape}}`
- **Response (success):** `{{shape}}`
- **Response (error):** `{{shape}}`
- **Auth:** _Required role or permission_

### 3.4 Data Changes (if applicable)

_New tables, columns, migrations, or seed data:_

---

## 4. Edge Cases & Error Handling

| Scenario | Expected Behavior | Error Response |
|----------|------------------|---------------|
| {{edge case}} | {{what should happen}} | {{error message or code}} |
| {{edge case}} | {{what should happen}} | {{error message or code}} |

---

## 5. Integration Points

_How this phase connects to work from prior phases or sets up future phases:_

### From Prior Phases
- **Phase {{N}}:** _What we depend on (specific tables, endpoints, auth flows)_

### For Future Phases
- **Phase {{N}}:** _What we're setting up (contracts, interfaces, data structures)_

---

## 6. Acceptance Criteria

_Checklist of conditions that must be true for this phase to be considered complete:_

- [ ] {{Criterion 1 — specific, testable}}
- [ ] {{Criterion 2 — specific, testable}}
- [ ] {{Criterion 3 — specific, testable}}
- [ ] All new code has tests with passing CI
- [ ] No regressions in prior phase functionality

---

## 7. Open Questions

_Anything unresolved that needs to be decided before or during implementation:_

- [ ] {{Question}}

---

## Revision History

| Date | Change | Reason |
|------|--------|--------|
| {{DATE}} | Initial spec | Created via `/bef deep-dive {{N}}` |
