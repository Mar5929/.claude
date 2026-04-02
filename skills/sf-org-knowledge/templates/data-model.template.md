# Data Model And Relationship Map

> **Org:** {{ORG_ALIAS}} | **Generated:** {{TIMESTAMP}}

## Overview

Total objects with mapped relationships: {{TOTAL_OBJECTS_MAPPED}}

## Entity Relationship Diagram

<!-- INSTRUCTION: Generate a Mermaid `erDiagram` from relationship fields in the describe files.
Use a readable subset if the full graph is too large, and write the same diagram to
`docs/org/data-model.mermaid`. -->

```mermaid
erDiagram
    {{ERD_DIAGRAM}}
```

> Standalone version: `docs/org/data-model.mermaid`

## Relationship Inventory

| Child Object | Field API Name | Parent Object | Relationship Type | Relationship Name |
| --- | --- | --- | --- | --- |
| {{RELATIONSHIP_ROWS}} |

## Object Clusters

<!-- INSTRUCTION: Identify tightly connected clusters to support domain discovery. -->

### {{CLUSTER_NAME}}

Objects: {{CLUSTER_OBJECTS}}

Key relationships:
{{CLUSTER_RELATIONSHIPS}}

## Junction Objects

| Junction Object | Parent 1 | Parent 2 | Notes |
| --- | --- | --- | --- |
| {{JUNCTION_ROWS}} |

## Self-Referencing Relationships

| Object | Field | Relationship Name |
| --- | --- | --- |
| {{SELF_REF_ROWS}} |

## Missing Data Notes

{{MISSING_DATA_NOTES}}
