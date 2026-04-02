# Integration Map

> **Org:** {{ORG_ALIAS}} | **Generated:** {{TIMESTAMP}}

## Summary

| Integration Type | Count |
| --- | --- |
| Connected Apps | {{CONNECTED_APP_COUNT}} |
| Named Credentials | {{NAMED_CREDENTIAL_COUNT}} |
| Remote Site Settings | {{REMOTE_SITE_COUNT}} |
| External Data Sources | {{EXTERNAL_DS_COUNT}} |
| Platform Event Channels | {{PE_CHANNEL_COUNT}} |

## Connected Apps

<!-- INSTRUCTION: Flag apps missing descriptions. -->

| App Name | Contact Email | Description |
| --- | --- | --- |
| {{CONNECTED_APP_ROWS}} |

## Named Credentials

<!-- INSTRUCTION: Flag any endpoint that starts with http:// as a risk. -->

| Name | Endpoint | Principal Type | Protocol |
| --- | --- | --- | --- |
| {{NAMED_CREDENTIAL_ROWS}} |

## Remote Site Settings

| Name | URL | Active |
| --- | --- | --- |
| {{REMOTE_SITE_ROWS}} |

## External Data Sources

| Name | Type | Description |
| --- | --- | --- |
| {{EXTERNAL_DS_ROWS}} |

## Platform Event Channels

| Channel Name | Description |
| --- | --- |
| {{PE_CHANNEL_ROWS}} |

## Integration Topology

<!-- INSTRUCTION: Provide a short text map of key connectivity patterns. If no integrations are detected,
write "No external integrations detected." -->

```text
{{INTEGRATION_TOPOLOGY}}
```

## Missing Data Notes

{{MISSING_DATA_NOTES}}
