# Domain Context

> Business concepts, entities, and domain events.
> Read this to understand the business logic.

---

## Business Domain

**Domain:** {e.g., E-commerce, Content Management, Analytics}
**Core Problem:** {What business problem does this solve?}

---

## Key Entities

### {Entity 1}
- **Definition:** {What is this entity?}
- **Lifecycle:** {Created → Active → Archived}
- **Key Fields:** {Important attributes}
- **Relationships:** {How it relates to other entities}

### {Entity 2}
- **Definition:** {What is this entity?}
- **Lifecycle:** {States and transitions}
- **Key Fields:** {Important attributes}
- **Relationships:** {How it relates to other entities}

---

## Domain Events

| Event | Trigger | Consumers | Side Effects |
|-------|---------|-----------|--------------|
| {EntityCreated} | {When triggered} | {Who listens} | {What happens} |
| {EntityUpdated} | {When triggered} | {Who listens} | {What happens} |
| {EntityDeleted} | {When triggered} | {Who listens} | {What happens} |

---

## Business Rules

### Invariants (Must Always Be True)
1. {Rule 1: e.g., "Every order must have at least one item"}
2. {Rule 2: e.g., "User email must be unique"}
3. {Rule 3}

### Constraints
1. {Constraint 1: e.g., "Maximum 100 items per order"}
2. {Constraint 2: e.g., "Discount cannot exceed 50%"}

### State Transitions
```
{Entity} States:
  DRAFT → PENDING → ACTIVE → COMPLETED
                  ↘ CANCELLED
```

---

## Metrics & KPIs

| Metric | Description | Target |
|--------|-------------|--------|
| {Metric 1} | {What it measures} | {Target value} |
| {Metric 2} | {What it measures} | {Target value} |

---

## Domain Terminology

See [glossary.md](./glossary.md) for complete terminology reference.
