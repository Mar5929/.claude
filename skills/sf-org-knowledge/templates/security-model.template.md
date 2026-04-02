# Security Model

> **Org:** {{ORG_ALIAS}} | **Generated:** {{TIMESTAMP}}

## Summary

| Component | Count |
| --- | --- |
| Standard Profiles | {{STANDARD_PROFILE_COUNT}} |
| Custom Profiles | {{CUSTOM_PROFILE_COUNT}} |
| Permission Sets | {{PERMISSION_SET_COUNT}} |
| Permission Set Groups | {{PSG_COUNT}} |
| Sharing Rules | {{SHARING_RULE_COUNT}} |

## Profiles

### Standard Profiles

| Profile Name |
| --- |
| {{STANDARD_PROFILE_ROWS}} |

### Custom Profiles

> Custom profiles should be minimized. Prefer Permission Sets and Permission Set Groups where possible.

| Profile Name |
| --- |
| {{CUSTOM_PROFILE_ROWS}} |

## Permission Sets

| Name | Label | Description | Custom | Namespace | Type |
| --- | --- | --- | --- | --- | --- |
| {{PERMISSION_SET_ROWS}} |

### Permission Sets Missing Descriptions

| Name | Label |
| --- | --- |
| {{PS_NO_DESC_ROWS}} |

## Permission Set Groups

<!-- INSTRUCTION: Include member permission sets when the source data provides them. -->

| Group Name | Label | Description | Included Permission Sets |
| --- | --- | --- | --- |
| {{PSG_ROWS}} |

## Sharing Rules

<!-- INSTRUCTION: Use `sharing-rules-summary.json` when available, otherwise fall back to metadata-list output. -->

| Rule Name | Object | Type |
| --- | --- | --- |
| {{SHARING_RULE_ROWS}} |

## Security Observations

{{SECURITY_OBSERVATIONS}}

## Missing Data Notes

{{MISSING_DATA_NOTES}}
