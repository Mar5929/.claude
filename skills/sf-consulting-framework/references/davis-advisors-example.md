# Davis Advisors — Starter Implementation

## Purpose

This document shows how the SF Consulting Framework maps to the Davis Advisors engagement. It provides pre-populated content for each framework file based on what we currently know. When initializing the Davis Advisors project with Claude Code, use this as the starting state.

---

## File-by-File Starter Content

### `.framework/config.yaml`

```yaml
client: Davis Advisors
engagement: Salesforce Enhancement — 3 Month Implementation
consulting_firm: CGI
start_date: 2025-01-15
target_end_date: 2025-04-15
current_phase: Discovery
primary_contact:
  name: John Driscoll
  role: Head of Marketing / Marketing-IT
  notes: Wears many hats — marketing, IT, Pardot admin. Primary decision-maker.
epics:
  - id: discovery-data-integration
    name: Discovery Data Integration (RIA)
    prefix: DDI
  - id: platform-roll-up-audit
    name: Platform Roll-Up Audit
    prefix: PRUP
  - id: data-loads-dakota-vetify
    name: One-Time FA Data Loads (Dakota & Vetify)
    prefix: DL
  - id: data-deduplication
    name: Data Deduplication
    prefix: DD
```

---

### `CLAUDE.md`

```markdown
# Davis Advisors: Salesforce Enhancement

## Quick Context

- **Client:** Davis Advisors
- **Engagement:** Salesforce Enhancement: 3 Month Implementation
- **Consulting firm:** CGI
- **Timeline:** 2025-01-15 to 2025-04-15
- **Current phase:** Discovery

## Key Files

| File | What It Contains |
|------|-----------------|
| `engagement/engagement-overview.md` | Davis business context, SF org state, stakeholders |
| `engagement/requirements.md` | Business requirements with epic traceability |
| `engagement/execution-plan.md` | Phase ordering and dependencies |
| `engagement/roadmap.md` | Timeline, milestones, completion status |
| `engagement/architecture/solution-design.md` | Cross-epic solution architecture |
| `.framework/config.yaml` | Project metadata and epic registry |

## Epics

| Epic | Prefix | Phase | Key Contact |
|------|--------|-------|-------------|
| Discovery Data Integration (RIA) | DDI | Not Started | John Driscoll |
| Platform Roll-Up Audit | PRUP | Discovery | John Driscoll |
| One-Time FA Data Loads (Dakota & Vetify) | DL | Not Started | John Driscoll |
| Data Deduplication | DD | Discovery | John Driscoll |

## Rules

Behavioral rules for this project live in `.claude/rules/framework-rules.md`. Read that file for instructions on how to handle questions, decisions, transcript processing, Linear sync, and all other framework operations.
```

---

### `engagement/engagement-overview.md`

```markdown
# Davis Advisors — Engagement Overview

## About the Client

Davis Advisors is a financial services company selling ETFs through financial advisors (FAs). They don't sell directly to end investors — their sales and marketing teams build relationships with FAs who then recommend Davis's ETF products.

Davis does not have a large internal IT team. John Driscoll (Head of Marketing) is the primary stakeholder and also functions as their marketing/IT person.

## Salesforce Org

- **Tenure:** ~10 years on Salesforce
- **Edition:** Sales Cloud only (no Financial Services Cloud)
- **Tech debt:** Significant — long-tenured org with accumulated customizations

### Key Objects
- **Contacts** = Financial Advisors (FAs)
- **Accounts** = Firms, Broker Dealers, RIAs
- **Leads** = Prospective FAs or firms
- **Platform Roll-Up** = Custom object — one record per FA with rolled-up financial transactions by product and time frame

### Key Tools
- **Pardot (MCAE)** — Email marketing to FAs. John manages campaigns, segmentation, sends.
- **Geopointe** — Geographic mapping for FA segmentation
- **Ascendix Search** — Advanced search/reporting layered on SF
- **Discovery** — External data vendor for FA movement data (e.g., advisor changes firms)
- **DemandTools** — Data quality / deduplication tool

## Current Business Processes

### Email Marketing
John uses Pardot to run campaigns targeting FAs. Workflow: set up campaign → pull in leads/contacts → create email → send mass email blasts.

### FA Segmentation & Reporting
Internal sales users segment FAs using Geopointe + Ascendix Search, filtering on:
- Rolled-up financial transactions (by product, by time frame — rolling 3-year, MTD, etc.)
- Geography
- Activity history (logged calls/meetings with FAs)

### Platform Roll-Up Batch Job
Legacy batch job runs every weekend:
1. Purges all existing Platform Roll-Up records
2. Rebuilds every record from scratch by aggregating raw transaction data
3. Takes approximately two full days to complete

Each record = one FA with transaction values aggregated by product and time frame.

## Scope

Four epics — see individual epic folders for details:
1. Discovery Data Integration (RIA)
2. Platform Roll-Up Audit
3. One-Time FA Data Loads (Dakota & Vetify)
4. Data Deduplication

## Previous Partner Context
Another SF consulting firm was engaged before CGI. They were responsible for incorporating partnership transactions into the Platform Roll-Up. Quality of their work is unknown — CGI's first task on that epic is to audit it.
```

---

### `engagement/questions.md` — Starter Questions

```markdown
# Questions — Engagement-Wide

## Open

### Q-ENG-001
**Asked:** 2025-01-15
**Owner:** Us
**Status:** Open
**Affects:** Data Deduplication, Data Loads, Platform Roll-Up Audit
**Blocks:** Pardot impact analysis across all data-modifying epics
**Question:** When we merge duplicate contacts, load new FAs, or modify roll-up data, what is the impact on existing Pardot segmentation lists? Does Pardot dynamically update segment membership, or do segments need to be rebuilt?

---

### Q-ENG-002
**Asked:** 2025-01-15
**Owner:** Client (John Driscoll)
**Status:** Open
**Affects:** All epics
**Question:** What is Davis's change management process for deploying Salesforce changes to production? Is there a sandbox → staging → prod pipeline, or do they deploy directly?

---

### Q-ENG-003
**Asked:** 2025-01-15
**Owner:** Client (John Driscoll)
**Status:** Open
**Affects:** All epics
**Question:** What are the Salesforce edition limits and API call limits for Davis's org? Are there governor limit concerns with the batch jobs and integrations we're planning?

---

## Answered

(None yet)

## Parked

(None yet)
```

---

### `engagement/execution-plan.md` — Initial Phasing

```markdown
# Execution Plan — Davis Advisors

**Last updated:** 2025-01-15
**Updated by:** Claude (initial setup from engagement kickoff)

## Epic Phasing

| Phase | Epic | Rationale | Status |
|-------|------|-----------|--------|
| 1 | Data Deduplication | Foundational — clean data before loading more. Dedup should happen before any data loads to prevent creating new duplicates. | Discovery |
| 1 | Platform Roll-Up Audit | Can run in parallel with Dedup. The audit is read-only analysis — doesn't modify data. | Discovery |
| 2 | Data Loads (Dakota & Vetify) | Depends on Dedup completing. Loading FA data into a dirty org risks creating duplicates that need re-merging. | Not Started |
| 2 | Discovery Data Integration (RIA) | Can potentially run in parallel with Data Loads. RIA advisors are a distinct dataset, but need to confirm no overlap with Dedup scope. | Not Started |

### Phase Dependencies
- Phase 2 start requires: Dedup merge execution complete in production
- Exception TBD: Discovery Data Integration may be independent — need to confirm RIA advisor overlap with existing data

### Open Phasing Questions
- Q-ENG-001 may affect phasing — if Pardot segments need manual rebuilding after dedup, that adds time between phases
- Platform Roll-Up Audit outcome may introduce a Phase 1.5 — if the previous partner's work needs to be scrapped and rebuilt, that's significant scope

---

## Task Execution

(Not yet broken down — epics are in Discovery phase. Task-level execution plans will be added as epics move into Design.)

---

## Blocked Items

| Item | Blocked By | Type | Owner |
|------|-----------|------|-------|
| Phase 2 start | Dedup production merge | Phase dependency | — |
| Pardot impact analysis | Q-ENG-001 | Question | Us |

---

## Changelog

| Date | Change | Triggered By |
|------|--------|-------------|
| 2025-01-15 | Initial epic phasing established | Engagement kickoff analysis |
```

---

### `engagement/requirements.md`: Starter Requirements

```markdown
# Requirements: Davis Advisors

**Last updated:** 2025-01-15

## Functional Requirements

### REQ-001
**Added:** 2025-01-15
**Source:** Engagement kickoff (John Driscoll)
**Priority:** Must Have
**Maps to:** data-deduplication
**Status:** Captured
**Requirement:** Duplicate FA contact records must be identified and merged before any new data loads. The surviving record must retain activity history from all merged records.

---

### REQ-002
**Added:** 2025-01-15
**Source:** Engagement kickoff (John Driscoll)
**Priority:** Must Have
**Maps to:** platform-roll-up-audit
**Status:** Captured
**Requirement:** The Platform Roll-Up batch job must correctly incorporate partnership transactions into FA roll-up records, including split percentage calculations.

---

### REQ-003
**Added:** 2025-01-15
**Source:** Engagement kickoff (John Driscoll)
**Priority:** Must Have
**Maps to:** data-loads-dakota-vetify
**Status:** Captured
**Requirement:** FA records from Dakota and Vetify data sources must be loaded into Salesforce as a one-time import with proper field mapping and deduplication against existing records.

---

### REQ-004
**Added:** 2025-01-15
**Source:** Engagement kickoff (John Driscoll)
**Priority:** Should Have
**Maps to:** discovery-data-integration
**Status:** Captured
**Requirement:** RIA advisor movement data from Discovery must integrate into Salesforce so the sales team can track when advisors change firms.

---

## Non-Functional Requirements

### REQ-005
**Added:** 2025-01-15
**Source:** Engagement kickoff (John Driscoll)
**Priority:** Should Have
**Maps to:** Engagement-wide
**Status:** Captured
**Requirement:** All data modifications (merges, loads, roll-up changes) must not break existing Pardot email segmentation lists.

---

## Deferred Requirements

(None yet)
```

---

### `engagement/roadmap.md`: Initial Roadmap

```markdown
# Roadmap: Davis Advisors

**Last updated:** 2025-01-15
**Updated by:** Claude (auto-maintained)

## Timeline

| Milestone | Target Date | Status | % Complete |
|-----------|-------------|--------|------------|
| Discovery complete | 2025-02-07 | In Progress | 10% |
| Design complete | 2025-02-28 | Not Started | 0% |
| Build complete | 2025-03-28 | Not Started | 0% |
| UAT complete | 2025-04-08 | Not Started | 0% |
| Go-live | 2025-04-15 | Not Started | 0% |

## Phase Status by Epic

| Epic | Discovery | Design | Build | Test | Deploy |
|------|-----------|--------|-------|------|--------|
| Data Deduplication | In Progress | -- | -- | -- | -- |
| Platform Roll-Up Audit | In Progress | -- | -- | -- | -- |
| Data Loads (Dakota & Vetify) | Not Started | -- | -- | -- | -- |
| Discovery Data Integration (RIA) | Not Started | -- | -- | -- | -- |

## Current Focus

Discovery phase for Data Deduplication and Platform Roll-Up Audit epics. Both running in parallel. Key open questions need client answers before design can begin.

## Upcoming Milestones

- **Discovery complete (2025-02-07):** All engagement-wide and Phase 1 epic questions answered. Technical designs drafted for Dedup and Roll-Up Audit.
- **Phase 1 Design complete (2025-02-14):** Technical designs approved. User stories generated. Ready for build.

## Changelog

| Date | Change | Triggered By |
|------|--------|-------------|
| 2025-01-15 | Roadmap initialized | Engagement kickoff |
```

---

### Epic Starter: `architecture/epics/data-deduplication/questions.md`

```markdown
# Questions — Data Deduplication

## Open

### Q-DD-001
**Asked:** 2025-01-15
**Owner:** Client (John Driscoll)
**Status:** Open
**Blocks:** [Technical Design — Merge Rules](./technical-design.md#merge-rules)
**Question:** How should we handle conflicting field values when merging duplicate FA contacts? For example, if two duplicate records have different phone numbers or email addresses, which record's values win?

---

### Q-DD-002
**Asked:** 2025-01-15
**Owner:** Us
**Status:** Open
**Blocks:** [Technical Design — Scope](./technical-design.md#scope)
**Question:** Are there FA contacts in Salesforce without CRD numbers? If so, how do we identify and handle duplicates for those records?

---

### Q-DD-003
**Asked:** 2025-01-15
**Owner:** Us
**Status:** Open
**Blocks:** [Technical Design — Merge Rules](./technical-design.md#merge-rules)
**Question:** When two duplicate FA contacts are merged, what happens to their associated activities (calls, meetings, tasks)? Does DemandTools preserve activity history from both records on the surviving record?

---

### Q-DD-004
**Asked:** 2025-01-15
**Owner:** Us
**Status:** Open
**Blocks:** [Technical Design — Pardot Impact](./technical-design.md#pardot-impact)
**Question:** When a Pardot prospect's underlying Salesforce contact is merged via DemandTools, does the Pardot prospect record update automatically? Or does it create orphaned Pardot records?

---

### Q-DD-005
**Asked:** 2025-01-15
**Owner:** Client (John Driscoll)
**Status:** Open
**Question:** What is the approximate volume of duplicate FA records? Do we have a rough count or percentage?

---

## Answered

(None yet)

## Parked

(None yet)
```

---

### Epic Starter: `architecture/epics/platform-roll-up-audit/questions.md`

```markdown
# Questions — Platform Roll-Up Audit

## Open

### Q-PRUP-001
**Asked:** 2025-01-15
**Owner:** Us
**Status:** Open
**Blocks:** [Technical Design — Audit Approach](./technical-design.md#audit-approach)
**Question:** What exactly did the previous consulting partner deliver? Do we have access to their code, documentation, or design specs? Or are we reverse-engineering from what's deployed in the org?

---

### Q-PRUP-002
**Asked:** 2025-01-15
**Owner:** Client (John Driscoll)
**Status:** Open
**Question:** Can we get access to sample partnership transaction records to understand the data model? We need to see the fields, the relationship to the partner FA, and the split percentage structure.

---

### Q-PRUP-003
**Asked:** 2025-01-15
**Owner:** Us
**Status:** Open
**Blocks:** [Technical Design — Partnership Logic](./technical-design.md#partnership-transaction-logic)
**Question:** In partnership transactions, how is the "split percentage" determined? Is it a field on the transaction record, or is it derived from a separate allocation table?

---

### Q-PRUP-004
**Asked:** 2025-01-15
**Owner:** Us
**Status:** Open
**Question:** The current batch job takes two full days (runs weekends). Is this acceptable to the business, or is performance improvement in scope?

---

### Q-PRUP-005
**Asked:** 2025-01-15
**Owner:** Us
**Status:** Open
**Blocks:** [Technical Design — Audit Criteria](./technical-design.md#audit-approach)
**Question:** What are our criteria for "the previous partner's work is acceptable" vs. "we need to scrap and redo"? We need to define this before the audit so the recommendation is objective.

---

## Answered

(None yet)

## Parked

(None yet)
```

---

### Summary of Starter Files to Generate

When initializing the Davis Advisors project, Claude Code should create:

```
davis-advisors/
├── CLAUDE.md                          ← Quick context for Claude Code
├── .claude/
│   └── rules/
│       └── framework-rules.md         ← From claude-md-template.md (with Davis-specific header)
├── .framework/
│   └── config.yaml                    ← Pre-populated above
├── engagement/
│   ├── engagement-overview.md         ← Pre-populated above
│   ├── questions.md                   ← Starter engagement-wide questions
│   ├── decisions.md                   ← Empty template
│   ├── risks.md                       ← Empty template
│   ├── execution-plan.md             ← Initial phasing above
│   ├── requirements.md               ← Starter requirements above
│   ├── roadmap.md                     ← Initial roadmap above
│   │
│   ├── meeting-notes/                 ← Empty, ready for meeting notes
│   │
│   └── architecture/
│       ├── solution-design.md         ← Empty template (to be populated during discovery)
│       │
│       └── epics/
│           ├── data-deduplication/
│           │   ├── overview.md            ← Epic context with prefix: DD
│           │   ├── questions.md           ← Starter questions above
│           │   ├── decisions.md           ← Empty template
│           │   ├── technical-design.md    ← Section stubs for merge rules, scope, pardot impact
│           │   └── user-stories.md        ← Empty template
│           ├── platform-roll-up-audit/
│           │   ├── overview.md            ← Epic context with prefix: PRUP
│           │   ├── questions.md           ← Starter questions above
│           │   ├── decisions.md           ← Empty template
│           │   ├── technical-design.md    ← Section stubs for audit approach, partnership logic
│           │   └── user-stories.md        ← Empty template
│           ├── data-loads-dakota-vetify/
│           │   ├── overview.md            ← Epic context with prefix: DL
│           │   ├── questions.md           ← Empty template
│           │   ├── decisions.md           ← Empty template
│           │   ├── technical-design.md    ← Empty template
│           │   └── user-stories.md        ← Empty template
│           └── discovery-data-integration/
│               ├── overview.md            ← Epic context with prefix: DDI
│               ├── questions.md           ← Empty template
│               ├── decisions.md           ← Empty template
│               ├── technical-design.md    ← Empty template
│               └── user-stories.md        ← Empty template
└── references/                        ← Empty, ready for transcripts and brain dumps
```
