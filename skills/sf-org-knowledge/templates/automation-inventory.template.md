# Automation Inventory

> **Org:** {{ORG_ALIAS}} | **Generated:** {{TIMESTAMP}}

## Summary

| Automation Type | Total | Active | Inactive |
| --- | --- | --- | --- |
| Flows (Record-Triggered) | {{RT_FLOW_TOTAL}} | {{RT_FLOW_ACTIVE}} | {{RT_FLOW_INACTIVE}} |
| Flows (Screen) | {{SCREEN_FLOW_TOTAL}} | {{SCREEN_FLOW_ACTIVE}} | {{SCREEN_FLOW_INACTIVE}} |
| Flows (Scheduled) | {{SCHED_FLOW_TOTAL}} | {{SCHED_FLOW_ACTIVE}} | {{SCHED_FLOW_INACTIVE}} |
| Flows (Autolaunched) | {{AUTO_FLOW_TOTAL}} | {{AUTO_FLOW_ACTIVE}} | {{AUTO_FLOW_INACTIVE}} |
| Flows (Platform Event) | {{PE_FLOW_TOTAL}} | {{PE_FLOW_ACTIVE}} | {{PE_FLOW_INACTIVE}} |
| Process Builder | {{PB_TOTAL}} | {{PB_ACTIVE}} | {{PB_INACTIVE}} |
| Workflow Rules | {{WF_TOTAL}} | {{WF_ACTIVE}} | {{WF_INACTIVE}} |
| Validation Rules | {{VR_TOTAL}} | {{VR_ACTIVE}} | {{VR_INACTIVE}} |

## Flow Inventory

<!-- INSTRUCTION: Use FlowDefinitionView data first, then metadata-list data where needed to fill gaps. -->

### Record-Triggered Flows

| Flow Name | Label | Object | Trigger Timing | Event | Status | Last Modified |
| --- | --- | --- | --- | --- | --- | --- |
| {{RT_FLOW_ROWS}} |

### Screen Flows

| Flow Name | Label | Description | Status | Last Modified |
| --- | --- | --- | --- | --- |
| {{SCREEN_FLOW_ROWS}} |

### Scheduled Flows

| Flow Name | Label | Description | Status | Last Modified |
| --- | --- | --- | --- | --- |
| {{SCHED_FLOW_ROWS}} |

### Autolaunched Flows

| Flow Name | Label | Description | Status | Last Modified |
| --- | --- | --- | --- | --- |
| {{AUTO_FLOW_ROWS}} |

### Platform Event Flows

| Flow Name | Label | Event | Status | Last Modified |
| --- | --- | --- | --- | --- |
| {{PE_FLOW_ROWS}} |

## Legacy Automation

> Process Builder and Workflow Rules are legacy automation types and should be reviewed for migration to Flow.

### Process Builder

| Process Name | Object | Status | Last Modified |
| --- | --- | --- | --- |
| {{PB_ROWS}} |

### Workflow Rules

| Rule Name | Object | Status |
| --- | --- | --- |
| {{WF_ROWS}} |

## Validation Rules

| Rule Name | Object | Active |
| --- | --- | --- |
| {{VR_ROWS}} |

## Automation By Object

<!-- INSTRUCTION: Combine Flow, Process Builder, Workflow Rule, Validation Rule, and trigger counts by object.
Flag objects with more than 5 total automations in notes or status text. -->

| Object | Flows | Process Builders | Workflow Rules | Validation Rules | Triggers | Total |
| --- | --- | --- | --- | --- | --- | --- |
| {{AUTOMATION_BY_OBJECT_ROWS}} |

## Missing Data Notes

{{MISSING_DATA_NOTES}}
