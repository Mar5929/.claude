# Apex Inventory

> **Org:** {{ORG_ALIAS}} | **Generated:** {{TIMESTAMP}}

**Note:** This doc inventories classes and triggers only. It must not analyze or summarize source logic.

## Summary

| Metric | Count |
| --- | --- |
| Apex Classes | {{TOTAL_CLASSES}} |
| Custom Classes | {{CUSTOM_CLASSES}} |
| Managed Package Classes | {{MANAGED_CLASSES}} |
| Apex Triggers | {{TOTAL_TRIGGERS}} |
| Test Classes | {{TEST_CLASS_COUNT}} |
| Classes On Older API Versions | {{OLD_API_COUNT}} |

## Apex Classes

<!-- INSTRUCTION: Group classes by inferred naming pattern. If a group is empty, write "None detected." -->

### Service Classes

| Class Name | API Version | Lines | Namespace | Status | Last Modified |
| --- | --- | --- | --- | --- | --- |
| {{SERVICE_CLASS_ROWS}} |

### Selector Classes

| Class Name | API Version | Lines | Namespace | Status | Last Modified |
| --- | --- | --- | --- | --- | --- |
| {{SELECTOR_CLASS_ROWS}} |

### Trigger Handlers

| Class Name | API Version | Lines | Namespace | Status | Last Modified |
| --- | --- | --- | --- | --- | --- |
| {{HANDLER_CLASS_ROWS}} |

### Domain Classes

| Class Name | API Version | Lines | Namespace | Status | Last Modified |
| --- | --- | --- | --- | --- | --- |
| {{DOMAIN_CLASS_ROWS}} |

### Test Classes

| Class Name | API Version | Lines | Status | Last Modified |
| --- | --- | --- | --- | --- |
| {{TEST_CLASS_ROWS}} |

### Batch Queueable Schedulable

| Class Name | Pattern | API Version | Lines | Status | Last Modified |
| --- | --- | --- | --- | --- | --- |
| {{ASYNC_CLASS_ROWS}} |

### Controllers

| Class Name | API Version | Lines | Namespace | Status | Last Modified |
| --- | --- | --- | --- | --- | --- |
| {{CONTROLLER_CLASS_ROWS}} |

### Other Classes

| Class Name | API Version | Lines | Namespace | Status | Last Modified |
| --- | --- | --- | --- | --- | --- |
| {{OTHER_CLASS_ROWS}} |

## Managed Package Namespaces

| Namespace | Class Count | Notes |
| --- | --- | --- |
| {{MANAGED_NAMESPACE_ROWS}} |

## Apex Triggers

<!-- INSTRUCTION: Show trigger event coverage with simple Yes/No or checkmark text. -->

| Trigger Name | Object | Before Insert | After Insert | Before Update | After Update | Before Delete | After Delete | After Undelete | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| {{TRIGGER_ROWS}} |

## Objects With Multiple Triggers

<!-- INSTRUCTION: Flag objects with more than 1 trigger because the preferred pattern is one trigger per object. -->

| Object | Trigger Count | Trigger Names |
| --- | --- | --- |
| {{MULTI_TRIGGER_ROWS}} |

## API Version Distribution

| API Version | Class Count | Status |
| --- | --- | --- |
| {{API_VERSION_ROWS}} |

## Missing Data Notes

{{MISSING_DATA_NOTES}}
