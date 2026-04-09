# Integration Map: {{PROJECT_NAME}}

> Last Updated: {{DATE}}

---

## Overview

_Summary of all external dependencies: third-party APIs, services, webhooks,
and any system-to-system integrations._

---

## External Services

### {{Service Name}} (e.g., Stripe)

| Attribute | Details |
|-----------|---------|
| **Purpose** | _Why we integrate with this service_ |
| **API Type** | REST / GraphQL / SDK / Webhook |
| **Base URL** | `{{url}}` |
| **Auth Method** | API Key / OAuth / JWT |
| **Key Endpoints Used** | _List the specific endpoints or SDK methods_ |
| **Rate Limits** | _Requests per second/minute/hour_ |
| **Webhook Events** | _Events we subscribe to, if any_ |
| **Sandbox/Test Mode** | _How to test without hitting production_ |
| **Cost Model** | _Per-request, subscription, usage tiers_ |

**Data Flow:**
```
Our System → [what we send] → Service → [what we receive] → Our System
```

**Failure Modes:**
- _What happens if this service is down?_
- _Retry strategy_
- _Fallback behavior_
- _User-facing impact_

**Implementation Notes:**
- _SDK vs direct API calls_
- _Wrapper/adapter pattern recommendations_

---

_(repeat for each external service)_

---

## Webhooks (Inbound)

| Source | Event | Endpoint | Processing | Retry Handling |
|--------|-------|----------|-----------|----------------|
| {{service}} | {{event_name}} | {{/api/webhooks/...}} | {{what we do with it}} | {{idempotency strategy}} |

---

## Webhooks (Outbound)

| Destination | Event | Trigger | Payload | Retry Policy |
|-------------|-------|---------|---------|-------------|
| {{service}} | {{event_name}} | {{what triggers it}} | {{shape}} | {{retries, backoff}} |

---

## Environment Variables Required

| Variable | Service | Description | Required In |
|----------|---------|-------------|-------------|
| {{VAR_NAME}} | {{service}} | {{what it is}} | dev / staging / prod |

---

## Integration Risk Assessment

| Service | Risk Level | Impact if Down | Mitigation |
|---------|-----------|---------------|------------|
| {{service}} | Low / Medium / High | {{what breaks}} | {{fallback plan}} |
