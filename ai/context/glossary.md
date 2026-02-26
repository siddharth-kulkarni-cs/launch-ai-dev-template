# Glossary

> Important terminology used in this repository.
> Reference this when encountering unfamiliar terms.

---

## Domain Terms

| Term | Definition |
|------|------------|
| {Term 1} | {Definition} |
| {Term 2} | {Definition} |
| {Term 3} | {Definition} |

---

## Technical Terms

| Term | Definition |
|------|------------|
| Controller | Handles HTTP requests, validates input, delegates to services |
| Service | Contains business logic, orchestrates operations |
| Repository | Data access layer, abstracts database operations |
| DTO | Data Transfer Object - shapes data between layers |
| Guard | Authorization check before request processing |
| Middleware | Cross-cutting concern applied to request pipeline |

---

## Acronyms

| Acronym | Expansion | Context |
|---------|-----------|---------|
| API | Application Programming Interface | REST endpoints |
| DTO | Data Transfer Object | Request/response shapes |
| ORM | Object-Relational Mapping | Database abstraction |
| DI | Dependency Injection | Service instantiation |

---

## Naming Conventions

| Pattern | Meaning | Example |
|---------|---------|---------|
| `*Service` | Business logic class | `OrderService` |
| `*Repository` | Data access class | `OrderRepository` |
| `*Controller` | HTTP handler class | `OrderController` |
| `*Guard` | Authorization check | `AuthGuard` |
| `*Middleware` | Request pipeline | `LoggingMiddleware` |
| `*Error` | Custom error class | `OrderNotFoundError` |
