# SF Consulting Framework — Question System

Questions are the atomic unit of discovery. The question system tracks every unknown from the moment it's raised through to resolution and downstream impact. It is the engine that drives the knowledge base and feeds the design hierarchy.

---

## Question Lifecycle

```
RAISED → SCOPED → OWNED → ANSWERED → IMPACTS ASSESSED
                                          │
                              ┌────────────┼────────────┐
                              ▼            ▼            ▼
                        Design updated  New questions  Work unblocked
                                        raised
```

1. **Raised** — A question is identified from any source: brain dump, meeting transcript, design review, build-phase discovery, or direct input.
2. **Scoped** — Claude determines whether the question is engagement-wide, epic-scoped, or feature-scoped, and files it in the appropriate `questions.md`.
3. **Owned** — An owner is assigned: you, the client, a teammate, or "TBD" if not yet determined.
4. **Answered** — The answer is recorded. Claude then performs the impact assessment.
5. **Impacts Assessed** — Claude checks:
   - Does this answer change any existing technical design? → Update the design doc.
   - Does this answer resolve a blocker on any user story or task? → Flag the item as unblocked.
   - Does this answer raise new questions? → Create them.
   - Does this answer contradict a previous decision? → Flag the conflict.

---

## Question ID Scheme

Format: `Q-{SCOPE}-{NUMBER}`

- `{SCOPE}` — A short prefix indicating where the question lives:
  - `ENG` — Engagement-wide
  - Epic prefix — A 2-4 letter abbreviation for the epic (defined in the epic's overview.md)
  - Feature prefix — `{EPIC}-{FEATURE}` for feature-scoped questions

- `{NUMBER}` — Simple incrementing number, zero-padded to 3 digits.

**Examples:**
- `Q-ENG-001` — Engagement-wide question #1
- `Q-DM-001` — Data Migration epic question #1
- `Q-DM-LRT-001` — Data Migration → Legacy Record Transformation feature question #1
- `Q-RA-001` — Report Audit epic question #1

Epic prefixes are defined in each epic's `overview.md` under a `prefix:` field.

---

## Question File Format

Each `questions.md` file uses this format:

```markdown
# Questions — {Scope Name}

## Open

### Q-DM-001
**Asked:** 2025-01-20
**Owner:** Client (Sarah Chen)
**Status:** Open
**Blocks:** [Technical Design — Merge Rules](./technical-design.md#merge-rules)
**Question:** How should we handle conflicting field values when merging duplicate records? (e.g., two records have different phone numbers — which wins?)

---

### Q-DM-002
**Asked:** 2025-01-20
**Owner:** Us
**Status:** Open
**Blocks:** [DM-US-003](./user-stories.md#dm-us-003)
**Question:** Are there records in the system without external IDs? If so, how do we identify and handle duplicates for those?

---

## Answered

### Q-DM-003
**Asked:** 2025-01-18
**Owner:** Client (Sarah Chen)
**Status:** Answered 2025-01-22
**Answer:** The vendor connector is pre-built — no custom development needed. The client already has an active subscription. We need to configure the connector and map the fields.
**Impact:**
- Updated [INT Technical Design](../integration-setup/technical-design.md) — removed custom API integration section, added connector configuration section.
- Unblocked [INT-US-001](../integration-setup/user-stories.md#int-us-001)
- New question raised: [Q-INT-002](../integration-setup/questions.md#q-int-002) — What fields does the connector sync by default?

---

## Parked

### Q-DM-010
**Asked:** 2025-01-25
**Owner:** TBD
**Status:** Parked — not relevant until build phase
**Question:** What is the license limit on batch operations for the merge tool?

---
```

### Sections

- **Open** — Active questions that need answers.
- **Answered** — Resolved questions with their answers and documented impact.
- **Parked** — Questions that are valid but not relevant yet. Revisit later.

### Required Fields

| Field | Required | Description |
|-------|----------|-------------|
| **ID** | Yes | Unique ID per the scheme above |
| **Asked** | Yes | Date the question was raised |
| **Owner** | Yes | Who is responsible for getting the answer. Use "Us", "Client ({name})", or a specific person. |
| **Status** | Yes | One of: Open, Answered {date}, Parked — {reason} |
| **Question** | Yes | The question itself, stated clearly enough that someone outside the project could understand it |
| **Blocks** | No | What work is waiting on this answer. Links to design sections, user stories, or tasks. |
| **Answer** | On answer | The answer, recorded when resolved |
| **Impact** | On answer | What changed as a result of the answer. Links to updated docs, unblocked items, new questions. |

---

## Cross-Cutting Questions

Some questions affect multiple epics. These live in `engagement/questions.md` and use the `Q-ENG-{NUMBER}` prefix.

Cross-cutting questions should include an **Affects** field listing the epics they touch:

```markdown
### Q-ENG-005
**Asked:** 2025-01-22
**Owner:** Us
**Status:** Open
**Affects:** Data Migration, Report Audit, Integration Setup
**Blocks:** Email segmentation design across all data-modifying epics
**Question:** When we merge duplicate records or load new data, what is the impact on existing email marketing segments? Do segments dynamically update, or do they need to be rebuilt?
```

---

## How Questions Get Raised

Claude accepts questions from any input format and handles the structuring:

### Direct Input
You say: "We need to figure out if email segments break when we merge duplicates."

Claude:
1. Creates `Q-ENG-005` (or appropriate scope)
2. Determines it's cross-cutting (affects multiple data-modifying epics)
3. Files it in `engagement/questions.md`
4. Links it to the relevant epics
5. Confirms back to you what it created

### Transcript Processing
You paste a meeting transcript. Claude:
1. Extracts all questions raised during the meeting
2. Extracts all answers given during the meeting
3. Extracts all decisions made
4. For each question: scopes it, assigns ownership based on context, files it
5. For each answer: finds the matching question (or creates one), records the answer, assesses impact
6. For each decision: records it with rationale, links to driving questions
7. Saves the raw transcript to `references/`
8. Gives you a summary of everything it extracted and filed

### Build-Phase Discovery
During build, a question comes up on a task. You tell Claude: "New question on the record merge task — what happens to activity history when we merge records?"

Claude applies the test:
- Does this change the design? → Creates a question in the knowledge base, links it to the task.
- Does this just unblock the task? → Notes it on the task/Linear ticket only.

---

## Linking Questions to Work Items

Questions connect to the rest of the system through the **Blocks** and **Impact** fields.

**Blocks** (set when question is open):
- Links to technical design sections that can't be finalized until the question is answered
- Links to user stories that can't be built until the question is answered
- Links to tasks that are waiting on the answer

**Impact** (set when question is answered):
- Links to design docs that were updated based on the answer
- Links to user stories or tasks that are now unblocked
- Links to new questions the answer raised
- Links to decisions that were made based on the answer

This creates the traceability chain. From any question, you can follow the links to see everything it affected. From any design decision, you can trace back to the question that drove it.

---

## Question Volume and File Sizing

For most consulting engagements, expect:
- 5-15 engagement-wide questions
- 10-30 questions per epic
- 5-15 questions per feature (if features exist)

At these volumes, a single `questions.md` per scope is manageable. If a particular scope exceeds ~50 questions, Claude can split into `questions-open.md` and `questions-resolved.md` to keep the active list scannable.

---

## The Morning Briefing — Questions View

When you ask "what's next?" or "give me a briefing," Claude synthesizes the question state across all scopes:

**Example briefing output:**

```
PROJECT STATUS: {Client Name}
================================

OPEN QUESTIONS: 14 total
  Engagement-wide: 3 (2 owned by client, 1 owned by us)
  Epic A: 5 (3 blocking design, 2 parked)
  Epic B: 2 (both blocking user stories)
  Epic C: 2 (1 blocking design, 1 parked)
  Epic D: 2 (both owned by us — actionable)

BLOCKED WORK:
  - Epic A Technical Design: merge rules section blocked on Q-EA-003 (client)
  - EB-US-001 and EB-US-002: blocked on Q-EB-001 (us — we need to investigate)

RECENTLY ANSWERED:
  - Q-EB-003 answered yesterday — vendor connector is pre-built. Design updated.
    → EB-US-001 is now unblocked.

NEXT ACTIONS:
  1. Investigate Q-EB-001 (we own it, it's blocking 2 stories)
  2. Follow up with client on Q-EA-003 (blocking design finalization)
  3. Both Q-ED-007 and Q-ED-008 are actionable — pick one to resolve
```
