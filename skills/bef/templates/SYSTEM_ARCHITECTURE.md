# System Architecture: {{PROJECT_NAME}}

> Last Updated: {{DATE}}

---

## Overview

_One paragraph describing the overall system topology: monolith vs microservices,
client-server model, deployment model, and the key architectural principles guiding decisions._

---

## System Diagram

_Text description of the high-level system topology. Describe the major components
and how they communicate._

```
[Client App] → [API Gateway / Server] → [Database]
                     ↓
              [External Services]
                     ↓
              [Background Workers]
```

---

## Component Architecture

### Component 1: {{Name}} (e.g., "Web Client")
- **Responsibility:** _What does this component do?_
- **Technology:** _What's it built with?_
- **Communication:** _How does it talk to other components? (REST, GraphQL, WebSocket, etc.)_
- **State Management:** _How does it manage state?_

### Component 2: {{Name}} (e.g., "API Server")
- **Responsibility:** _What does this component do?_
- **Technology:** _What's it built with?_
- **API Surface:** _What endpoints/interfaces does it expose?_
- **Authentication:** _How are requests authenticated?_

_(repeat for each major component)_

---

## API Design

### API Style
_REST / GraphQL / RPC / Hybrid — and why_

### Endpoint Conventions
- Base URL: `{{base_url}}`
- Versioning: _Strategy (URL path, header, etc.)_
- Response format: _Standard envelope (e.g., `{ data, error, meta }`)_
- Error format: _Standard error shape_
- Pagination: _Strategy (cursor, offset, etc.)_

### Key Endpoints (High-Level)
| Method | Endpoint | Purpose |
|--------|----------|---------|
| {{GET}} | {{/api/v1/resource}} | {{Description}} |

---

## Authentication & Authorization

### Authentication
- **Strategy:** _e.g., JWT, session-based, OAuth 2.0_
- **Token lifecycle:** _Issuance, refresh, expiration_
- **Storage:** _Where tokens are stored client-side_

### Authorization
- **Model:** _RBAC, ABAC, ACL, etc._
- **Roles:** _List of roles and their permissions_
- **Enforcement:** _Where authorization checks happen (middleware, service layer, etc.)_

---

## State Management

### Client State
- _Approach: Redux, Zustand, React Query, etc._
- _What lives in client state vs. what's always fetched_

### Server State
- _Session management approach_
- _Caching strategy and invalidation_

---

## Error Handling Strategy

- **Client errors (4xx):** _How are validation errors structured and returned?_
- **Server errors (5xx):** _How are unexpected errors caught and reported?_
- **Retry strategy:** _Which operations are idempotent and retryable?_
- **Logging:** _What gets logged and where?_

---

## Background Processing

_If applicable: job queues, scheduled tasks, async workflows._

| Job | Trigger | Purpose | Failure Handling |
|-----|---------|---------|-----------------|
| {{job_name}} | {{trigger}} | {{purpose}} | {{what happens on failure}} |

---

## Security Architecture

- **Transport:** _TLS, certificate management_
- **Data at rest:** _Encryption approach_
- **Secrets management:** _How are API keys, DB credentials stored?_
- **Input validation:** _Strategy and where it's enforced_
- **CORS policy:** _Allowed origins, methods_
- **Rate limiting:** _Strategy and thresholds_
