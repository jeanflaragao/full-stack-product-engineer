# Engineering Principles

## Purpose

This document defines the engineering principles that guide architectural decisions throughout the Full Stack Product Engineer journey.

These principles are not a collection of mandatory design patterns.

They exist to guide engineering judgment.

A principle should help answer:

> "What should we optimize for when making this decision?"

Architectural patterns should be introduced only when they solve a concrete problem or create a deliberate learning opportunity.

---

# 1. Architecture Before Implementation

Important engineering problems should be understood before implementation begins.

Before writing significant code, consider:

- what problem are we solving?
- what behavior is required?
- where does that responsibility belong?
- what boundaries exist?
- what alternatives are available?
- what trade-offs are involved?
- how will the behavior be tested?
- how will the system behave when it fails?

The objective is not to predict the future perfectly.

The objective is to make intentional decisions rather than allowing architecture to emerge accidentally from implementation details.

---

# 2. Business Rules Belong to the Appropriate Boundary

Business rules should live in the layer responsible for enforcing them.

Controllers should not become repositories for business rules.

Models should not become containers for unrelated application workflows.

Services should not become generic dumping grounds for arbitrary logic.

A business rule should have a clear owner.

For example:

```text
HTTP concerns
    ↓
Controller

Application orchestration
    ↓
Application Service

Business behavior
    ↓
Domain Model / Domain Service

Persistence
    ↓
Repository / Active Record
```

The exact structure may vary according to the problem.

The important principle is explicit responsibility.

---

# 3. Controllers Are Application Boundaries

Controllers translate HTTP concerns into application operations.

A controller should primarily be responsible for:

- receiving the request;
- authenticating the caller;
- loading the appropriate resource;
- authorizing access;
- invoking application behavior;
- translating the result into an HTTP response.

Controllers should avoid becoming the place where business workflows are implemented.

A useful heuristic is:

> If the logic would still exist if HTTP disappeared, it probably does not belong in the controller.

---

# 4. Services Orchestrate Application Behavior

Application services represent meaningful application operations.

They should coordinate the work required to execute a use case.

A service should:

- express a meaningful operation;
- coordinate collaborators;
- enforce application-level workflow;
- preserve transactional boundaries where appropriate;
- expose a clear success/failure contract.

Services should not automatically be introduced for every model operation.

The existence of a service should communicate meaningful application behavior.

---

# 5. Do Not Abstract Prematurely

Abstractions have a cost.

Before introducing an abstraction, ask:

- what problem does it solve?
- how often does the problem occur?
- what complexity does the abstraction introduce?
- does it make the system easier to understand?
- does it establish a useful boundary?
- would duplication actually be harmful?

Prefer the simplest design that clearly satisfies the current requirements.

Abstraction should follow demonstrated need rather than anticipation.

---

# 6. Explicit Boundaries Over Implicit Behavior

Important boundaries should be visible in the architecture.

Examples include:

- authentication boundaries;
- authorization boundaries;
- transaction boundaries;
- service boundaries;
- error boundaries;
- tenant boundaries;
- integration boundaries.

A boundary should make it clear:

- who owns the responsibility;
- what enters the boundary;
- what leaves the boundary;
- how failures behave.

Implicit boundaries make systems harder to reason about.

---

# 7. Errors Are Part of the Application Contract

Failures should be designed as deliberately as successful behavior.

The application should distinguish between different classes of failure.

Examples include:

```text
Business/Application Failure
        ↓
Known application exception

Resource Failure
        ↓
Record not found / not visible

Authorization Failure
        ↓
Policy denial

System Failure
        ↓
Unexpected infrastructure or programming failure
```

The application should not treat every exception as equivalent.

Known failures should have predictable behavior.

Unexpected failures should not be silently converted into business errors.

---

# 8. Services Should Not Hide Collaborator Failures

A service should not rescue an exception merely because it can.

When a collaborating service raises an exception, the default behavior should be propagation.

A service should rescue an exception only when that exception is explicitly part of its own business responsibility.

Conceptually:

```text
Collaborator
     ↓
raises known failure
     ↓
Application Service
     ↓
propagate
     ↓
Application Boundary
     ↓
translate into API contract
```

This prevents services from becoming accidental exception translators and keeps failure ownership explicit.

This principle is formally captured in ADR-0002.

---

# 9. Resource Visibility Is a Security Concern

Resource lookup and authorization are not merely implementation details.

The API must consider whether a response can reveal information about resources the caller should not be able to observe.

For example, when a resource belongs to another user, returning:

`403 Forbidden`

may reveal that the resource exists.

In contexts where existence itself should not be disclosed, the system should return:

`404 Not Found`

The distinction between 404 and 403 should therefore be intentional.

---

# 10. Authorization Is Not Authentication

Authentication answers:

> Who are you?

Authorization answers:

> Are you allowed to perform this operation on this resource?

These concerns should remain separate.

The system should authenticate the caller before performing protected operations and then explicitly authorize access to the relevant resource.

Pundit policies and policy scopes are used to establish these boundaries in the current application.

---

# 11. Query Complexity Should Be Isolated

Read-side filtering, searching, sorting, and pagination can create increasingly complex query logic.

When that complexity becomes meaningful, it should be isolated from controllers.

The project uses Query Objects to compose read behavior.

For example:

```text
IndexQuery
    ├── FilterQuery
    ├── SearchQuery
    └── SortQuery
```

The purpose is not to create a Query Object for every database query.

The purpose is to keep complex read behavior composable, testable, and understandable.

---

# 12. API Contracts Should Be Explicit

An API should have predictable response semantics.

Successful responses and failed responses should follow explicit contracts.

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

Clients should not need to understand internal implementation details to interpret API responses.

---

# 13. Tests Should Protect Behavior

Tests exist to provide confidence in system behavior.

The goal is not maximizing test count.

Tests should protect:

- business behavior;
- API contracts;
- authorization;
- failure behavior;
- important integration paths;
- regressions.

A test should answer a meaningful question about the system.

When architecture changes, tests should help verify that the externally meaningful behavior remains correct.

---

# 14. Architecture Must Be Observable

A production system should not only work.

Engineers should be able to understand what the system is doing.

As the project evolves, observability should provide appropriate visibility through:

- structured logs;
- metrics;
- traces;
- health checks;
- error tracking;
- operational dashboards.

Observability should be introduced according to operational needs rather than as decorative infrastructure.

---

# 15. Reliability Is a Feature

Production systems must account for failure.

Engineering decisions should consider:

- retries;
- idempotency;
- transactions;
- timeouts;
- partial failures;
- duplicate execution;
- recovery;
- operational visibility.

A feature is not production-ready merely because its happy path works.

---

# 16. Complexity Must Be Justified

Complexity is sometimes necessary.

Distributed systems, event-driven architecture, CQRS, caching, background processing, and other advanced techniques can solve real problems.

But every additional mechanism introduces:

- cognitive cost;
- operational cost;
- testing cost;
- failure modes;
- maintenance cost.

Therefore:

> Introduce complexity when its benefits justify its costs.

The project should demonstrate the ability to recognize when a sophisticated pattern is appropriate and when it is unnecessary.

---

# 17. Prefer Evolutionary Architecture

Architecture should evolve with the system.

The project should avoid trying to design the final architecture before the real requirements are understood.

The expected progression is:

```text
Simple solution
      ↓
Real requirement
      ↓
Observed constraint
      ↓
Architectural pressure
      ↓
Deliberate evolution
```

Architectural evolution should be supported by:

- tests;
- ADRs;
- observability;
- incremental changes.

---

# 18. Architecture Decisions Must Be Documented

Important architectural decisions should be recorded in ADRs.

An ADR should explain:

- context;
- problem;
- decision;
- alternatives;
- consequences;
- trade-offs.

The purpose is to preserve reasoning.

The code tells us what the system does.

The ADR tells us why it does it.

---

# 19. Historical Decisions Should Not Be Erased

The project is an engineering journey.

Previous decisions may later prove to be incomplete, incorrect, or inappropriate.

That does not make them worthless.

Historical decisions provide evidence of how engineering understanding evolved.

When an architectural decision changes:

- preserve the original ADR;
- create a new decision when appropriate;
- explain the reason for the change.

Do not rewrite history merely to make the project appear cleaner.

---

# 20. Security Is Part of Architecture

Security should not be treated as a final checklist.

Architectural decisions should consider:

- authentication;
- authorization;
- resource visibility;
- tenant isolation;
- information leakage;
- secrets;
- input validation;
- rate limiting;
- auditability.

Security implications should be considered when boundaries are designed.

---

# 21. Operational Concerns Belong in Design

Architecture should consider how the system will behave in production.

For significant features, ask:

- how will this be monitored?
- how will it fail?
- how will it recover?
- how will it be deployed?
- how will it be rolled back?
- how will operators diagnose problems?
- what happens during partial failure?

Production behavior is part of the design.

---

# 22. Product Thinking Matters

Engineering decisions should be connected to product outcomes.

A technically elegant implementation that does not solve the user's problem is not a successful product decision.

Engineers should consider:

- user value;
- business rules;
- usability;
- operational workflows;
- reliability;
- cost;
- time to delivery.

Technical quality and product value should be balanced deliberately.

---

# 23. AI Must Increase Engineering Leverage

AI tools are part of the engineering workflow, but they do not replace engineering judgment.

AI should help with:

- exploration;
- implementation;
- testing;
- code review;
- documentation;
- architecture analysis;
- investigation.

The engineer remains responsible for:

- understanding the problem;
- validating generated work;
- making architectural decisions;
- evaluating trade-offs;
- accepting responsibility for the resulting system.

The goal is:

```text
Engineer + AI
      ↓
Greater engineering leverage
```

not:

```text
AI-generated code
      ↓
Unexamined system
```

---

# 24. Context Must Precede Significant Changes

Before making significant changes, an engineer or AI agent should understand the existing system.

The expected workflow is:

```text
Read context
    ↓
Understand current state
    ↓
Identify constraints
    ↓
Design change
    ↓
Implement
    ↓
Test
    ↓
Review
    ↓
Document
```

Agents should read:

- project instructions;
- current state;
- relevant ADRs;
- roadmap;
- affected code;
- relevant tests.

Context is part of engineering correctness.

---

# 25. Small, Reviewable Changes

Prefer changes that are:

- focused;
- incremental;
- easy to review;
- easy to test;
- easy to revert.

Large changes should be decomposed when possible.

A small change makes it easier to understand:

- what changed;
- why it changed;
- what could have broken;
- how to validate it.

---

# 26. Code Review Is Engineering

Code review is not merely a quality gate.

It is an opportunity to evaluate:

- correctness;
- architecture;
- maintainability;
- security;
- testing;
- observability;
- operational behavior;
- future consequences.

The project should progressively develop the ability to review code from a Staff Engineer perspective.

---

# 27. Explainability Is a Quality Attribute

An engineer should be able to explain important decisions clearly.

For significant architectural decisions, the engineer should be able to answer:

- What problem were we solving?
- What alternatives did we consider?
- Why did we choose this approach?
- What trade-offs did we accept?
- What could make us change the decision?

If a design cannot be explained, it probably has not been understood deeply enough.

---

# 28. The Simplest Correct Design Wins

When multiple designs satisfy the requirements, prefer the design that is easiest to:

- understand;
- test;
- operate;
- modify;
- explain.

Sophistication is not the objective.

Engineering effectiveness is.

---

# Applying These Principles

These principles should be used as a decision framework rather than a checklist.

When making an architectural decision:

1. Identify the actual problem.
2. Identify the relevant boundaries.
3. Consider the simplest viable solution.
4. Identify alternatives.
5. Evaluate trade-offs.
6. Consider security and operational consequences.
7. Implement incrementally.
8. Test the behavior.
9. Document significant decisions.
10. Revisit the decision when the system provides new evidence.

The goal of the project is not to prove that these principles are always correct.

The goal is to develop the judgment required to know:

> When to apply them, when to adapt them, and when to deliberately violate them.
