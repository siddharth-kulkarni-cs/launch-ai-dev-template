# Architecture Overview

> System architecture, service boundaries, and data flow.
> Read this before making structural changes.

---

## System Overview

{Describe the high-level architecture of this repository}

**Type:** {Monolith / Microservice / Library / CLI / etc.}
**Primary Function:** {What this system does}
**Key Consumers:** {Who/what uses this system}

---

## Service Boundaries

```
┌─────────────────────────────────────────────────────────────┐
│                     External Clients                         │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      API Gateway                             │
│                   (Auth, Rate Limiting)                      │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Application Layer                         │
│              Controllers → Services → Repositories           │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                     Data Layer                               │
│                  Database / Cache / Queue                    │
└─────────────────────────────────────────────────────────────┘
```

---

## Data Flow

### Request Flow
1. Request enters via {entry point}
2. Passes through {middleware/guards}
3. Routed to {controller}
4. Business logic in {service}
5. Data access via {repository}
6. Response formatted and returned

### Event Flow
{Describe async event flows if applicable}

---

## External Dependencies

| Dependency | Purpose | Critical? |
|------------|---------|-----------|
| {Database} | Primary data store | Yes |
| {Cache} | Performance optimization | No |
| {Queue} | Async processing | Yes |
| {External API} | {Purpose} | {Yes/No} |

---

## Critical Paths

**Do not modify without careful review:**
- {Path 1: e.g., Authentication flow}
- {Path 2: e.g., Payment processing}
- {Path 3: e.g., Data migration scripts}

---

## Deployment Architecture

{Describe how this system is deployed}

- **Environment:** {Cloud provider, container orchestration}
- **Scaling:** {Horizontal/Vertical, auto-scaling rules}
- **Regions:** {Multi-region considerations}
