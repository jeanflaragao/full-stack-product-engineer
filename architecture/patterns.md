# Architecture Patterns

## Purpose

This document records architectural and engineering patterns relevant to the Full Stack Product Engineer journey.

Patterns are tools, not rules.

A pattern should be introduced because it solves a meaningful problem, establishes a useful boundary, or provides a deliberate engineering learning opportunity.

The presence of a pattern in this document does not mean that it must be used in the application.

Patterns should always be evaluated against the current problem, constraints, complexity, and operational requirements.

---

# Pattern Status

Patterns in this document use three classifications:

| Status | Meaning |
|---|---|
| Currently Used | Evidence exists that the pattern is currently used in the application |
| Studied / Explored | The pattern has been intentionally studied or discussed during the engineering journey |
| Planned | The pattern is part of the future engineering roadmap |

A pattern should not be marked as "Currently Used" without evidence.

---

# 1. Service Object

**Status: Currently Used**

## Problem

Application workflows can become difficult to reason about when implemented directly inside controllers or models.

## Purpose

A Service Object represents a meaningful application operation and coordinates the behavior required to execute that operation.

## Current Application Usage

The application currently uses module-namespaced services such as:

```ruby
Bookmakers::CreateService
Bookmakers::DestroyService
```

The service convention is:

`Klass.call(...)`

delegating to:

`#call`

## Appropriate Use

Use a service when an operation represents meaningful application behavior or requires coordination between multiple responsibilities.

## Avoid

Do not create a service merely to move one line of model code into another class.

The abstraction should communicate meaningful behavior.

---

# 2. Query Object

**Status: Currently Used**

## Problem

Filtering, searching, sorting, and other read-side concerns can make controllers and models difficult to understand.

## Purpose

Query Objects isolate complex read behavior and make query composition explicit.

## Current Application Usage

The bookmaker index uses:

```text
Bookmakers::IndexQuery
    ↓
FilterQuery
    ↓
SearchQuery
    ↓
SortQuery
```

This allows read-side behavior to be composed independently.

## Appropriate Use

Use Query Objects when query behavior becomes sufficiently complex that isolation improves:

- readability;
- composability;
- testability;
- maintainability.

## Avoid

Do not create a Query Object for every trivial database query.

---

# 3. Policy Object

**Status: Currently Used**

## Problem

Authorization logic can become scattered across controllers and models.

## Purpose

Policy Objects centralize authorization decisions around a resource and its actions.

## Current Application Usage

The application uses Pundit policies including:

- `ApplicationPolicy`
- `BookmakerPolicy`

Authorization is combined with policy scopes for resource visibility.

## Appropriate Use

Use policy objects when authorization rules need to be explicit, testable, and separated from HTTP concerns.

---

# 4. Policy Scope

**Status: Currently Used**

## Problem

Authorization is not only about whether an action is permitted.

It can also determine which records a caller is allowed to see.

## Purpose

Policy scopes constrain resource collections according to authorization rules.

The application uses policy scopes to establish ownership boundaries.

## Important Security Property

Resource lookup should respect visibility boundaries before operating on the resource.

This supports the project's intentional distinction between:

```text
Resource visible
    ↓
Authorization decision
```

and:

```text
Resource not visible
    ↓
404 Not Found
```

---

# 5. Application Error Boundary

**Status: Currently Used**

## Problem

Different parts of an application may raise different exceptions, while the API needs a consistent failure contract.

## Purpose

An application error boundary translates known application failures into a stable external API contract.

Conceptually:

```text
Application code
      ↓
Typed exception
      ↓
Application boundary
      ↓
Error handler
      ↓
API error response
```

## Current Application Usage

The application uses this architecture through:

- `ApplicationError`;
- typed subclasses;
- `ErrorHandler`;
- centralized `rescue_from`;
- request-level translation of authentication and resource-not-found failures into structured API responses.

Day 37 request specs verify the externally observable `401 Unauthorized` and structured `404 Not Found` behavior. This is evidence for the boundary itself, but not for the broader application-service failure contract described by ADR-0002.

## Appropriate Use

Use an explicit error boundary when internal failure mechanisms should not leak into the external API contract.

---

# 6. Application Service Failure Contract

**Status: Studied / Explored**

## Purpose

Known application exceptions normally propagate through services until an appropriate application boundary translates them. A service rescues only when it has a specific business reason to recover or translate, and services remain independent of HTTP.

## Current Evidence

ADR-0002 records this as a Proposed decision. Day 37 removed `Bookmakers::ActiveAccountsExistError` because no Account domain or active-account invariant exists. The application therefore has no real business workflow yet that validates service-to-HTTP propagation of a known application exception.

The active-account scenario is a deferred example for reconsideration after the Account domain exists, not current production behavior.

---

# 7. Layered Architecture

**Status: Currently Used**

The current application can be described as using a lightweight layered structure:

```text
HTTP / Controller
        ↓
Application Service
        ↓
Active Record / Domain Behavior
        ↓
Database
```

Cross-cutting responsibilities such as authorization and query composition establish additional boundaries.

The architecture is intentionally pragmatic rather than a strict implementation of a named architecture framework.

---

# 8. Serializer

**Status: Currently Used**

## Problem

Internal models should not automatically define the external representation of an API.

## Purpose

Serializers establish an explicit representation boundary between application objects and API responses.

## Current Application Usage

The application uses:

`BookmakerSerializer`

for bookmaker API representations.

## Appropriate Use

Use serializers when API representations need to remain explicit and independent from internal model structure.

---

# 9. API Response Contract

**Status: Currently Used**

## Problem

Inconsistent API responses make clients harder to implement and maintain.

## Purpose

A response contract establishes predictable success and failure semantics.

The current application uses:

```json
{
  "data": {},
  "meta": {}
}
```

for successful responses and:

```json
{
  "error": {
    "code": "machine_readable_code",
    "message": "human_readable_message"
  }
}
```

for failures.

The contract also defines intentional HTTP semantics for:

- 404;
- 403.

The contract is documented in ADR-0001.

---

# 10. Repository / Active Record Boundary

**Status: Studied / Explored**

## Problem

Persistence mechanisms can become tightly coupled to application behavior.

## Purpose

A Repository pattern can establish an abstraction between application behavior and persistence mechanisms.

Rails Active Record already provides substantial persistence behavior.

Therefore, introducing a Repository layer automatically may create abstraction without sufficient benefit.

## Engineering Position

The project does not assume that a Repository layer is required.

The decision should depend on actual architectural pressure.

This is an example of applying the principle:

> Do not abstract prematurely.

---

# 11. Dependency Injection

**Status: Studied / Explored**

## Problem

Hard-coded dependencies can make components difficult to test, replace, or reason about.

## Purpose

Dependency Injection makes collaborators explicit and allows implementations to be substituted.

## Appropriate Use

Dependency injection becomes particularly useful when:

- integrations are involved;
- infrastructure dependencies need replacement;
- testing requires controlled collaborators;
- multiple implementations exist.

## Engineering Position

Dependency Injection should be introduced when dependency management becomes a meaningful problem rather than as ceremony.

---

# 12. Transaction Script

**Status: Studied / Explored**

## Problem

Some application operations are naturally expressed as a sequence of coordinated steps.

## Purpose

A Transaction Script organizes a business operation as an explicit procedural workflow.

It can be appropriate for:

- simple use cases;
- CRUD-oriented workflows;
- operations where behavior does not justify richer domain modeling.

## Risk

Transaction Scripts can become large procedural services when business complexity increases.

The pattern should therefore be evaluated against the complexity of the domain.

---

# 13. Domain Model

**Status: Studied / Explored**

## Problem

Complex business behavior can become difficult to maintain when represented only through procedural application services.

## Purpose

A Domain Model places meaningful business behavior and invariants close to the concepts they represent.

## Engineering Position

The project should introduce richer domain modeling when the business rules justify it.

The goal is not to turn every Active Record model into a highly abstract domain object.

---

# 14. Domain Service

**Status: Studied / Explored**

## Problem

Some business rules involve multiple domain concepts and do not naturally belong to a single entity.

## Purpose

A Domain Service represents domain behavior that requires multiple domain objects or does not naturally belong to one entity.

## Appropriate Use

Use when a genuine domain concept exists but assigning the behavior to one entity would create an unnatural responsibility.

## Avoid

Do not use Domain Services as generic containers for logic that simply does not fit elsewhere.

---

# 15. Hexagonal Architecture

**Status: Planned / Studied**

## Problem

Business logic can become tightly coupled to frameworks, databases, and external services.

## Purpose

Hexagonal Architecture establishes a boundary between application/domain behavior and external adapters.

Conceptually:

```text
             External Systems
                    │
                 Adapters
                    │
                    ▼
        ┌─────────────────────┐
        │ Application / Domain│
        │       Core          │
        └─────────────────────┘
                    ▲
                 Adapters
                    │
             External Systems
```

## Engineering Goal

The project should study Hexagonal Architecture as a way to reason about dependency direction and external boundaries.

It should not be introduced mechanically.

---

# 16. Clean Architecture

**Status: Studied / Explored**

## Problem

Large applications can accumulate dependencies that make business logic increasingly difficult to isolate.

## Purpose

Clean Architecture emphasizes dependency direction toward policies and business rules.

## Engineering Position

The project studies Clean Architecture as a set of architectural ideas rather than a requirement to reproduce a specific folder structure.

The useful concepts include:

- dependency direction;
- boundaries;
- separation of concerns;
- independent business rules.

---

# 17. Onion Architecture

**Status: Studied / Explored**

## Problem

Application logic can become coupled to infrastructure concerns.

## Purpose

Onion Architecture organizes the system around a domain/application core with dependencies pointing inward.

The key concept is:

```text
Infrastructure
      ↓
Application
      ↓
Domain
```

The inner layers should not depend on outer infrastructure concerns.

## Engineering Position

The project studies Onion Architecture primarily as a way to reason about dependency direction and boundaries.

---

# 18. Domain-Driven Design

**Status: Planned / Studied**

## Problem

Complex business domains require models that reflect the language and structure of the business.

## Purpose

DDD provides concepts and techniques for modeling complex domains.

Relevant concepts include:

- bounded contexts;
- entities;
- value objects;
- aggregates;
- domain services;
- domain events;
- ubiquitous language.

## Engineering Position

DDD should be applied when domain complexity justifies it.

The project should not introduce DDD terminology without corresponding domain problems.

---

# 19. Domain Events

**Status: Planned**

## Problem

One business operation may need to communicate that something meaningful happened without tightly coupling all consumers to the originating operation.

## Purpose

A Domain Event represents a meaningful occurrence in the domain.

Example:

- `AccountCreated`
- `BookmakerDeactivated`
- `BetSettled`
- `WithdrawalCompleted`

Events can allow downstream behavior to react without directly coupling all participants.

## Risks

Events introduce complexity around:

- ordering;
- delivery;
- duplication;
- consistency;
- observability;
- failure recovery.

They should therefore be introduced deliberately.

---

# 20. Outbox Pattern

**Status: Planned**

## Problem

Updating a database and publishing an event are separate operations.

A failure between them can create inconsistency.

## Purpose

The Outbox Pattern stores events in the same database transaction as the business change.

Conceptually:

```text
Database Transaction
       │
       ├── Business State
       │
       └── Outbox Event
                │
                ▼
        Event Publisher
                │
                ▼
       External Consumer
```

## Engineering Goal

The project should study the pattern as a mechanism for reliable event publication.

---

# 21. CQRS

**Status: Planned**

## Problem

Read and write workloads can have different requirements.

## Purpose

CQRS separates command-side behavior from query-side behavior.

Conceptually:

```text
Commands
   ↓
Write Model

Queries
   ↓
Read Model
```

## Engineering Position

The project should introduce CQRS only when the difference between read and write requirements creates sufficient architectural pressure.

Separating every query and command from the beginning would create unnecessary complexity.

---

# 22. Event-Driven Architecture

**Status: Planned**

## Problem

Highly coupled synchronous systems can become difficult to scale and evolve.

## Purpose

Event-driven architecture allows components to communicate through events.

Conceptually:

```text
Producer
   ↓
Event
   ↓
Broker
   ├── Consumer A
   ├── Consumer B
   └── Consumer C
```

## Engineering Considerations

Important concerns include:

- delivery guarantees;
- idempotency;
- ordering;
- retries;
- observability;
- eventual consistency.

---

# 23. Saga

**Status: Planned**

## Problem

Distributed workflows cannot always rely on a single database transaction.

## Purpose

A Saga coordinates a distributed business workflow through a sequence of local transactions and compensating actions.

Conceptually:

```text
Step A
  ↓
Step B
  ↓
Step C

Failure
  ↓
Compensating actions
```

## Engineering Goal

Study Saga as a mechanism for managing distributed business workflows.

The project should distinguish between:

- orchestration;
- choreography.

---

# 24. Background Job

**Status: Planned**

## Problem

Some work does not need to execute during the request lifecycle.

## Purpose

Background jobs move asynchronous work outside the request-response cycle.

Examples include:

- notifications;
- imports;
- reports;
- reconciliation;
- event processing.

## Engineering Considerations

Background jobs require explicit thinking about:

- retries;
- idempotency;
- failure handling;
- observability;
- scheduling.

---

# 25. Idempotency

**Status: Planned / Studied**

## Problem

Retries and duplicate requests can cause the same operation to execute more than once.

## Purpose

An idempotent operation produces the same intended business result when safely repeated.

This becomes particularly important for:

- financial operations;
- external API calls;
- background jobs;
- event consumers.

## Engineering Principle

Idempotency should be designed around the business operation rather than implemented as a generic technical checkbox.

---

# 26. Feature Flag

**Status: Planned**

## Problem

Deploying code and exposing functionality do not always need to happen simultaneously.

## Purpose

Feature flags separate deployment from feature activation.

They can support:

- gradual rollout;
- experimentation;
- operational rollback;
- migration strategies.

## Risks

Feature flags introduce lifecycle complexity and should have ownership and removal strategies.

---

# 27. Observability Patterns

**Status: Planned**

Production systems require visibility into their behavior.

Relevant patterns include:

- structured logging;
- metrics;
- distributed tracing;
- correlation identifiers;
- health checks;
- dashboards;
- alerting.

Observability should answer:

- What happened?
- Why did it happen?
- How often does it happen?
- How long does it take?
- Which component is responsible?

---

# Pattern Selection

Patterns should be selected based on problems, not fashion.

A useful decision process is:

```text
Identify problem
      ↓
Understand constraints
      ↓
Consider simplest solution
      ↓
Evaluate patterns
      ↓
Compare complexity
      ↓
Choose deliberately
      ↓
Validate with implementation
```

A pattern should earn its place in the architecture.

---

# Pattern Evolution

A pattern may move between statuses as the project evolves.

For example:

```text
Studied
   ↓
Experimented
   ↓
Adopted
   ↓
Validated
```

Adoption should be based on evidence.

If a pattern later proves unnecessary, the project should be willing to remove it.

Architectural maturity includes knowing when to stop using a pattern.
