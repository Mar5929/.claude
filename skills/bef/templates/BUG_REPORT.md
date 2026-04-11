# BUG-{{ID}}: {{descriptive title based on investigation findings}}

- **Severity:** {{CRITICAL / HIGH / MEDIUM / LOW}}
- **Phase:** {{which BEF phase this relates to, inferred from the affected feature, or "N/A"}}
- **Status:** Open

## Description

{{Detailed description of the bug based on investigation. Include:
- What the user reported
- What was actually observed in the browser
- The scope of the impact (one page? multiple features?)}}

## Steps to Reproduce

1. {{Exact steps taken in Playwright to reproduce}}
2. {{Include URLs, buttons clicked, data entered}}
3. {{Note what happens at each step}}

## Expected Behavior

{{What should happen based on PRD/spec/common sense}}

## Actual Behavior

{{What actually happens, with evidence}}

## Evidence

### Console Errors
{{Paste relevant console errors/warnings, or "None observed"}}

### Failed Network Requests
{{List failed API calls with status codes and error responses, or "None observed"}}

### DOM Issues
{{Note any DOM anomalies found, or "None observed"}}

## Location

- {{file:line — primary source of the bug, from stack traces or code reading}}
- {{file:line — secondary affected files}}

## Root Cause

{{Analysis of why this is happening in the code}}

## Fix

{{Suggested fix approach based on root cause analysis}}

## Related Files

- {{List of files involved}}
