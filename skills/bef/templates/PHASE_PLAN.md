# Phase Plan: {{PROJECT_NAME}}

> Last Updated: {{DATE}}
> Total Phases: {{N}}
> Estimated Complexity: {{S/M/L/XL}}

---

## Dependency Graph

_Visual representation of phase ordering and parallelism:_

```
Phase 1 (Database Schema)
  ├── Phase 2 (Auth System)
  │     └── Phase 3 (Core API)
  │           └── Phase 5 (Advanced Features)
  └── Phase 4 (Frontend Shell)  ← can run in parallel with Phase 2
```

---

## Phase Summary

### Phase 1: {{Phase Name}}

| Attribute | Details |
|-----------|---------|
| **Scope** | _One paragraph describing what this phase builds_ |
| **Depends On** | _None / Phase numbers_ |
| **Unlocks** | _Phase numbers that can start after this completes_ |
| **Parallel With** | _Phase numbers that can run concurrently, if any_ |
| **Complexity** | S / M / L / XL |
| **Estimated Tasks** | _Rough count: 5-8, 8-12, etc._ |

**Key Deliverables:**
- _Deliverable 1_
- _Deliverable 2_

---

### Phase 2: {{Phase Name}}

| Attribute | Details |
|-----------|---------|
| **Scope** | _One paragraph_ |
| **Depends On** | Phase 1 |
| **Unlocks** | _Phase numbers_ |
| **Parallel With** | _Phase numbers or "None"_ |
| **Complexity** | S / M / L / XL |
| **Estimated Tasks** | _Rough count_ |

**Key Deliverables:**
- _Deliverable 1_
- _Deliverable 2_

---

_(repeat for each phase)_

---

## Execution Order Summary

_Clean ordered list showing the critical path and parallelism:_

1. **Phase 1:** {{Name}} — _must complete first_
2. **Phase 2:** {{Name}} + **Phase 4:** {{Name}} — _can run in parallel_
3. **Phase 3:** {{Name}} — _after Phase 2_
4. **Phase 5:** {{Name}} — _after Phase 3_

---

## Risk Notes

_Any phases with high uncertainty, external dependencies, or known complexity spikes:_

- **Phase {{N}}:** _Risk description and mitigation_
