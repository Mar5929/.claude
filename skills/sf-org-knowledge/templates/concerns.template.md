# Org Concerns And Observations

> **Org:** {{ORG_ALIAS}} | **Generated:** {{TIMESTAMP}}
> **Total Concerns:** {{TOTAL_CONCERNS}} ({{CRITICAL_COUNT}} critical, {{WARNING_COUNT}} warning, {{INFO_COUNT}} info)

## Severity Legend

- `CRITICAL`: immediate governor-limit, performance, or security risk
- `WARNING`: near-term debt or maintainability issue
- `INFO`: useful observation that should influence planning

## Governor Limit Risks

<!-- INSTRUCTION: Detect objects over 3,000,000 records, objects with more than 5 triggers,
and classes over 2,000 lines. -->

| Severity | Category | What Was Detected | Recommendation |
| --- | --- | --- | --- |
| {{GOVERNOR_ROWS}} |

## Dead Metadata

<!-- INSTRUCTION: Detect custom objects with zero records, inactive flows, and package-orphaned Apex. -->

| Severity | Category | What Was Detected | Recommendation |
| --- | --- | --- | --- |
| {{DEAD_METADATA_ROWS}} |

## Technical Debt

<!-- INSTRUCTION: Detect old API versions, Process Builder, Workflow Rules, and trigger sprawl. -->

| Severity | Category | What Was Detected | Recommendation |
| --- | --- | --- | --- |
| {{TECH_DEBT_ROWS}} |

## Integration Risks

<!-- INSTRUCTION: Detect non-HTTPS named credentials and Connected Apps missing descriptions. -->

| Severity | Category | What Was Detected | Recommendation |
| --- | --- | --- | --- |
| {{INTEGRATION_RISK_ROWS}} |

## Security Gaps

<!-- INSTRUCTION: Detect permission sets missing descriptions and custom profiles. -->

| Severity | Category | What Was Detected | Recommendation |
| --- | --- | --- | --- |
| {{SECURITY_GAP_ROWS}} |

## Data Quality Indicators

<!-- INSTRUCTION: Detect objects with more than 100 custom fields and major record-count outliers. -->

| Severity | Category | What Was Detected | Recommendation |
| --- | --- | --- | --- |
| {{DATA_QUALITY_ROWS}} |

## Notes

{{ADDITIONAL_NOTES}}
