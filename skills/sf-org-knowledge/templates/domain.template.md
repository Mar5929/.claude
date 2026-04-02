# Domain: {{DOMAIN_NAME}}

> {{DOMAIN_DESCRIPTION}}

## Key Objects

| Object | Role In Domain | Record Count | Custom Fields |
| --- | --- | --- | --- |
| {{DOMAIN_OBJECT_ROWS}} |

## Relationships

<!-- INSTRUCTION: Limit this section to domain-local relationships only. -->

```mermaid
erDiagram
    {{DOMAIN_ERD}}
```

| Parent | Child | Field | Type |
| --- | --- | --- | --- |
| {{DOMAIN_RELATIONSHIP_ROWS}} |

## Automation

| Type | Name | Object | Status |
| --- | --- | --- | --- |
| {{DOMAIN_AUTOMATION_ROWS}} |

## Integrations

| Integration | Type | Endpoint Or Detail |
| --- | --- | --- |
| {{DOMAIN_INTEGRATION_ROWS}} |

## Apex Touchpoints

<!-- INSTRUCTION: List only the classes and triggers associated with domain objects. Do not summarize code logic. -->

| Type | Name | Object | API Version |
| --- | --- | --- | --- |
| {{DOMAIN_APEX_ROWS}} |

## Relevant Concerns

| Severity | What Was Detected | Recommendation |
| --- | --- | --- |
| {{DOMAIN_CONCERN_ROWS}} |

## Notes

{{DOMAIN_NOTES}}
