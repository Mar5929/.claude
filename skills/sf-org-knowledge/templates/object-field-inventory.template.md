# Object And Field Inventory

> **Org:** {{ORG_ALIAS}} | **Generated:** {{TIMESTAMP}}

## Summary

| Metric | Count |
| --- | --- |
| Standard Objects With Customization | {{STANDARD_CUSTOMIZED_COUNT}} |
| Custom Objects | {{CUSTOM_OBJECT_COUNT}} |
| Managed Package Objects | {{MANAGED_OBJECT_COUNT}} |
| Total Custom Fields | {{TOTAL_CUSTOM_FIELD_COUNT}} |

## Custom Objects

<!-- INSTRUCTION: Sort by record count descending where available. -->

| Object API Name | Label | Record Count | Custom Fields | Relationships | Namespace |
| --- | --- | --- | --- | --- | --- |
| {{CUSTOM_OBJECT_ROWS}} |

## Standard Objects With Custom Fields

<!-- INSTRUCTION: Include standard objects only when describes show custom fields or notable relationships. -->

| Object API Name | Label | Custom Fields Added | Relationship Fields | Record Count |
| --- | --- | --- | --- | --- |
| {{STANDARD_OBJECT_ROWS}} |

## Managed Package Objects

<!-- INSTRUCTION: Group by namespace when managed-package objects are present. -->

| Namespace | Object API Name | Label | Record Count |
| --- | --- | --- | --- |
| {{MANAGED_OBJECT_ROWS}} |

## Object Detail

<!-- INSTRUCTION: Create one subsection per custom object and per materially customized standard object.
If data is missing, include a brief "Data Not Available" note instead of omitting the object. -->

### {{OBJECT_LABEL}} (`{{OBJECT_API_NAME}}`)

> Record Count: {{RECORD_COUNT}} | Custom Fields: {{CUSTOM_FIELD_COUNT}} | Deployment Status: {{DEPLOYMENT_STATUS}}

**Object Notes**

{{OBJECT_NOTES}}

| Field API Name | Label | Type | Required | Description |
| --- | --- | --- | --- | --- |
| {{FIELD_ROWS}} |

**Relationships**

| Field | Related To | Type | Relationship Name |
| --- | --- | --- | --- |
| {{RELATIONSHIP_ROWS}} |

## Objects With No Records

<!-- INSTRUCTION: List custom objects with zero records. If none, say so explicitly. -->

| Object API Name | Label | Deployment Status | Notes |
| --- | --- | --- | --- |
| {{EMPTY_OBJECT_ROWS}} |

## Missing Data Notes

{{MISSING_DATA_NOTES}}
