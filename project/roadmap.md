# Project Roadmap

## Purpose

This roadmap defines the current evolution of the Full Stack Product Engineer journey and the Betting Operations Management Platform.

It is intentionally capability-based rather than calendar-based.

The original project foundation contained a Month 1–5 timeline. That timeline remains preserved in `project/foundation.md` as historical context.

This document represents the current roadmap and may evolve as the project develops.

---

# Scope and Authority

This document is the **journey-level engineering roadmap**. It is not the authoritative implementation roadmap for the `betting-platform` application.

The two roadmaps track different things.

**Engineering Memory Repository (this roadmap)**

Tracks:

- engineering capabilities
- learning progression
- architectural maturity
- milestone progression

**`betting-platform` repository**

Tracks:

- product features
- implementation priorities
- technical tasks
- application delivery

The `betting-platform` repository's `ROADMAP.md` is authoritative for application implementation sequencing. This document does not copy or duplicate that roadmap.

Where the milestones below mention specific technologies — Rails, Sidekiq, event-driven architecture, CQRS, and similar — they describe engineering capabilities to be developed, not a commitment that those technologies will be implemented in the application at a specific point in time. Whether and when a given technology is actually introduced into `betting-platform`, and in what order, is determined by that repository's own roadmap and implementation priorities.

---

# Roadmap Philosophy

The project evolves through progressively increasing engineering complexity.

The progression is:

```text
Foundations
    ↓
Application Architecture
    ↓
Production Readiness
    ↓
Scalability
    ↓
AI-Native Product Engineering
```

Each milestone should introduce complexity only when it creates a meaningful engineering problem or learning opportunity.

The objective is not to complete a checklist of technologies.

The objective is to develop the ability to reason about increasingly complex systems.

---

# Milestone 0 — Foundations

**Status:** Completed

Establish the development environment and repository foundations.

## Capabilities

- project structure
- Dockerized PostgreSQL
- Rails API skeleton
- development environment
- Git workflow
- Conventional Commits
- basic CI foundation
- deployment foundation
- project documentation

## Engineering Evidence

The project should demonstrate the ability to establish a reproducible development environment and a professional repository foundation.

---

# Milestone 1 — Rails API Foundations

**Status:** Completed

Build the initial application capabilities and establish a reliable API foundation.

## Capabilities

- REST API
- authentication
- JWT
- authorization
- Pundit
- pagination
- query objects
- filtering
- searching
- sorting
- serialization
- request specs
- factories
- error handling
- API response contracts

## Core Domain

The initial domain centers around bookmaker account management.

The system supports concepts including:

- users
- bookmakers
- bookmaker ownership
- accounts
- account-related operations

The architecture establishes ownership and authorization boundaries around these resources.

## Engineering Evidence

The project should demonstrate:

- API design
- authentication
- authorization
- resource ownership
- query composition
- testing
- serialization
- structured error responses

---

# Milestone 2 — Application Architecture

**Status:** In Progress

Improve the internal architecture of the application and establish clear responsibilities between layers.

## Focus Areas

- service layer
- controller responsibilities
- application services
- domain behavior
- exception architecture
- failure boundaries
- API design
- testing philosophy
- code review culture
- architecture documentation

## Current Architectural Work

Important areas already explored include:

- API response contracts
- application service failure contracts
- business-rule exceptions
- ApplicationError
- RecordNotFound handling
- resource visibility
- authorization behavior
- information leakage
- controller/service boundaries

## Engineering Objective

Move from:

```text
Controller
    ↓
Model
```

toward deliberate application boundaries where responsibilities are explicit.

The objective is not to introduce abstractions for their own sake.

The objective is to make business behavior, failure handling, and application orchestration understandable and testable.

---

# Milestone 3 — Production Readiness

**Status:** Planned

Introduce the operational capabilities required for a production-grade application.

## Focus Areas

### Reliability

- database transactions
- failure recovery
- idempotency
- retries
- timeouts

### Background Processing

- Sidekiq
- asynchronous workflows
- job failure handling
- retry strategies
- idempotent jobs

### Observability

- structured logging
- metrics
- distributed tracing
- error tracking
- health checks
- dashboards

### Delivery

- CI/CD
- deployment automation
- environment management
- production configuration

### Operational Engineering

- feature flags
- audit logs
- rate limiting
- operational documentation

## Engineering Objective

Move from:

> The application works.

toward:

> The application can be operated reliably.

---

# Milestone 4 — Scaling and Distributed Architecture

**Status:** Planned

Introduce architectural techniques required when the system's scale and complexity justify them.

## Focus Areas

- domain events
- event-driven architecture
- asynchronous workflows
- outbox pattern
- CQRS
- caching
- performance optimization
- database optimization
- scalability
- multi-tenancy
- API versioning

## Event-Driven Evolution

The architecture should evolve progressively.

A typical evolution may be:

```text
Synchronous application behavior
            ↓
Domain events
            ↓
Asynchronous processing
            ↓
Reliable event publication
            ↓
Outbox pattern
            ↓
Distributed workflows
```

The exact implementation should be driven by actual requirements and deliberate engineering experiments.

## Engineering Objective

Develop the ability to reason about:

- consistency
- availability
- asynchronous processing
- event delivery
- failure recovery
- data ownership
- distributed system trade-offs

---

# Milestone 5 — AI-Native Product Engineering

**Status:** Planned

Integrate AI into the engineering workflow as a first-class engineering capability.

## Focus Areas

- AI-assisted development
- engineering agents
- code review agents
- architecture agents
- automated documentation
- AI-assisted testing
- AI-assisted investigation
- agent workflows
- project memory
- context management
- AI-native product features

## Engineering Objective

Learn how to use AI to increase engineering leverage without outsourcing engineering judgment.

The goal is:

```text
Engineer
    +
AI
    ↓
Higher engineering leverage
```

not:

```text
AI
    ↓
Engineer becomes dependent on generated code
```

---

# Cross-Cutting Engineering Capabilities

These capabilities evolve throughout all milestones.

## Testing

- unit testing
- request testing
- integration testing
- authorization testing
- failure testing
- regression protection
- testing strategy

## Security

- authentication
- authorization
- tenant isolation
- information leakage
- secure error handling
- secrets management
- security boundaries

## Observability

- logs
- metrics
- traces
- errors
- health checks
- operational dashboards

## Architecture

- modularity
- boundaries
- coupling
- cohesion
- dependency direction
- architectural trade-offs
- ADRs

## Product Engineering

- requirements
- business rules
- domain modeling
- product trade-offs
- user workflows
- operational workflows

## Communication

- technical documentation
- pull requests
- code review
- architecture discussions
- technical English
- interview communication

---

# Learning Loop

Every milestone follows the same engineering learning loop:

```text
Learn
  ↓
Design
  ↓
Implement
  ↓
Test
  ↓
Review
  ↓
Operate
  ↓
Observe
  ↓
Reflect
  ↓
Document
  ↓
Improve
```

The roadmap is therefore not simply a sequence of technologies.

It is a sequence of increasingly difficult engineering problems.

---

# Milestone Completion Criteria

A milestone should not be considered complete merely because its code has been implemented.

Completion requires evidence across multiple dimensions:

## Implementation

The capability exists in the application.

## Testing

The relevant behavior is covered by meaningful automated tests.

## Architecture

Important architectural decisions are understood and documented.

## Operations

Where applicable, the capability can be observed and operated.

## Documentation

The relevant engineering knowledge has been preserved.

## Communication

The engineer can explain the design and its trade-offs.

---

# Roadmap Evolution

This roadmap is intentionally mutable.

When the project discovers new requirements, architectural constraints, or learning opportunities, the roadmap may change.

However:

- completed milestones should remain historical;
- significant changes should be documented;
- obsolete plans should not be silently erased;
- major roadmap changes should be explained in project memory.

The roadmap should reflect the current direction while preserving the history of how that direction evolved.

---

# Current Direction

The current focus is:

> Milestone 2 — Application Architecture

The immediate objective is to deepen the application's service boundaries, exception architecture, API contracts, testing philosophy, and engineering review practices before moving into production-readiness concerns.

The next major transition is:

```text
Application Architecture
        ↓
Production Readiness
```

The project should not move forward simply because the calendar says it is time.

It should move forward when the current engineering problems have been sufficiently explored and the next level of complexity is justified.

---

# Long-Term Direction

The long-term trajectory is:

```text
Rails API Foundations
        ↓
Application Architecture
        ↓
Production Engineering
        ↓
Distributed Systems
        ↓
AI-Native Product Engineering
```

The ultimate objective is to develop the capability to design, build, operate, and evolve production-grade SaaS systems while being able to clearly explain the engineering decisions behind them.
