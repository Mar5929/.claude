# Data Model: {{PROJECT_NAME}}

> Last Updated: {{DATE}}

---

## Overview

_One paragraph describing the data architecture approach: relational vs document,
multi-tenancy strategy, soft delete policy, audit trail approach, etc._

---

## Entity Relationship Summary

_High-level description of the major entities and how they relate. A text-based
ERD description that could be used to generate a diagram._

```
User (1) ──── (N) Post
User (1) ──── (N) Comment
Post (1) ──── (N) Comment
```

---

## Entities

### {{EntityName}}

_Brief description of what this entity represents._

| Field | Type | Constraints | Description |
|-------|------|------------|-------------|
| id | UUID | PK, auto-generated | Primary identifier |
| {{field}} | {{type}} | {{constraints}} | {{description}} |
| created_at | Timestamp | NOT NULL, default now() | Record creation time |
| updated_at | Timestamp | NOT NULL, auto-update | Last modification time |

**Indexes:**
- `idx_{{entity}}_{{field}}` on `({{field}})` — _Reason for this index_

**Relationships:**
- Has many {{RelatedEntity}} via `{{foreign_key}}`
- Belongs to {{RelatedEntity}} via `{{foreign_key}}`

**Business Rules:**
- _Any constraints or rules that aren't captured in the schema_

---

_(repeat the Entity section for each entity)_

---

## Enumerations

### {{EnumName}}
| Value | Description |
|-------|-------------|
| {{value}} | {{description}} |

---

## Migration Strategy

_How will the schema be created and evolved?_
- Migration tool: {{e.g., Prisma Migrate, Knex, Alembic}}
- Seed data approach: {{description}}
- Rollback strategy: {{description}}

---

## Data Integrity Rules

_Cross-entity constraints, cascading behaviors, and business rules that
span multiple entities:_

1. {{Rule description}}
2. {{Rule description}}
