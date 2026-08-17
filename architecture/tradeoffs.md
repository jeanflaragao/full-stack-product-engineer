# Architectural Trade-offs

## Purpose

Architecture is fundamentally about trade-offs.

Most meaningful engineering decisions do not have a universally correct answer.

Different approaches optimize for different properties:

- simplicity;
- flexibility;
- performance;
- reliability;
- security;
- maintainability;
- delivery speed;
- operational complexity.

This document records the trade-offs encountered during the Full Stack Product Engineer journey.

The purpose is to preserve engineering reasoning rather than provide universal rules.

---

# 1. 404 Not Found vs 403 Forbidden

## The Question

When a user requests a resource they are not allowed to access, should the API return:

```text
404 Not Found
```

or:

```text
403 Forbidden
```

### 403

A 403 Forbidden response communicates that:

- the resource exists;
- the server understood the request;
- the caller is not authorized.

**Advantage**

It is semantically explicit about authorization.

**Risk**

It can reveal that a resource exists.

That may expose information that the caller should not be able to discover.

### 404

A 404 Not Found response communicates that the resource is not available to the caller.

**Advantage**

It can prevent resource existence from being disclosed.

**Risk**

It may hide the distinction between:

- a resource that does not exist;
- a resource that exists but is not visible.

## Current Project Position

The project intentionally uses 404 when resource visibility should not reveal the existence of another user's resource.

Authorization may still produce 403 when the resource is known to be visible and the caller is authenticated but not permitted to perform the requested action.

The distinction is therefore contextual rather than absolute.

## Engineering Lesson

HTTP status codes are also information channels.

Authorization design should consider information leakage, not merely semantic correctness.

---

# 2. Controller vs Service

## The Question

Where should application behavior live?

Possible approaches include:

- Controller

or:

- Application Service

### Controller-Centric Approach

The controller performs most of the workflow.

**Advantages**

- simple for very small applications;
- fewer abstractions;
- easy to follow initially.

**Risks**

Controllers can become responsible for:

- business rules;
- orchestration;
- transactions;
- error handling;
- persistence.

This makes them increasingly difficult to maintain and test.

### Service-Oriented Approach

The controller delegates meaningful application operations to services.

```text
HTTP Request
     ↓
Controller
     ↓
Application Service
     ↓
Domain / Persistence
```

**Advantages**

- clearer application boundaries;
- reusable application operations;
- easier unit testing;
- reduced controller complexity.

**Risks**

Services can become:

- procedural dumping grounds;
- excessively granular;
- difficult-to-understand abstractions.

## Current Project Position

The project uses services for meaningful application operations.

Controllers remain responsible for HTTP concerns.

The existence of a service should communicate a meaningful use case rather than simply move arbitrary code out of a controller.

## Engineering Lesson

"Thin controllers" is not sufficient architecture by itself.

The important question is:

> Where does the responsibility belong?

---

# 3. Service vs Model

## The Question

When business behavior involves an Active Record model, should the behavior live:

- inside the model

or:

- inside an application service?

### Model-Centric Approach

Behavior lives close to the data and domain object.

**Advantages**

- strong cohesion around the entity;
- easy access to state;
- simple for entity-level invariants.

**Risks**

Models can accumulate:

- persistence logic;
- validations;
- workflows;
- integrations;
- unrelated business processes.

### Service-Centric Approach

The service coordinates application behavior while the model maintains its own state and invariants.

**Advantages**

- clearer application workflows;
- easier orchestration;
- better separation of concerns.

**Risks**

A service can become an anemic abstraction if it merely forwards calls to the model.

## Current Project Position

The project favors explicit application services for meaningful workflows while keeping entity-level behavior and invariants close to the model/domain concept.

The distinction is responsibility-driven rather than ideological.

## Engineering Lesson

The question should not be:

> "Should business logic always be in services?"

The better question is:

> "Who owns this behavior?"

---

# 4. Rescue vs Propagation

## The Question

When a collaborating component raises an exception, should the current service rescue it?

### Rescue

The service catches the exception and translates or handles it.

**Advantages**

- local control;
- potentially simpler caller behavior;
- can recover from known failures.

**Risks**

- hides failure ownership;
- duplicates error translation;
- makes service composition harder;
- can accidentally swallow important failures.

### Propagation

The service allows the known exception to move upward.

```text
Collaborator
     ↓
Exception
     ↓
Service
     ↓
Application Boundary
```

**Advantages**

- explicit failure ownership;
- centralized translation;
- easier service composition;
- less duplicated error handling.

**Risks**

- requires a well-defined application boundary;
- callers must understand the propagated failure contract.

## Current Project Position

The default is propagation.

A service should rescue an exception only when that failure is explicitly part of its own responsibility.

This is the core decision captured by ADR-0002.

## Engineering Lesson

An exception should be handled by the boundary that owns the meaning of that failure.

---

# 5. Application Error vs StandardError

## The Question

Should application services raise typed application exceptions or allow generic exceptions to represent business failures?

### Generic Exceptions

Using generic exceptions such as StandardError provides little semantic information.

**Advantage**

- minimal implementation effort.

**Risks**

- callers cannot distinguish expected business failures from programming errors;
- error translation becomes ambiguous;
- APIs may accidentally expose implementation failures.

### Typed Application Errors

The application defines explicit exception types representing known application failures.

Examples include:

- `ApplicationError`
- `AuthenticationError`
- `ValidationError`

**Advantages**

- explicit failure semantics;
- predictable API translation;
- easier testing;
- clearer ownership.

**Risks**

- more types to maintain;
- poor naming can create unnecessary complexity;
- not every exception should become an application error.

## Current Project Position

`ApplicationError` represents known application/business failures.

Unexpected system and programming failures should not automatically be converted into application errors.

## Engineering Lesson

Typed errors are useful when they communicate meaningful application semantics.

They are not useful merely because they are more abstract.

---

# 6. Policy Scope vs Direct Lookup

## The Question

Should a resource be loaded directly:

```ruby
Bookmaker.find(params[:id])
```

or through an authorization-aware scope?

```ruby
policy_scope(Bookmaker).find(params[:id])
```

### Direct Lookup

**Advantages**

- simple;
- explicit;
- familiar Rails code.

**Risks**

Authorization may happen too late.

The system may reveal the existence of a resource before determining whether it should be visible.

### Scoped Lookup

The resource is first constrained by visibility.

```text
Policy Scope
     ↓
Visible Resources
     ↓
Find Resource
     ↓
Authorize Action
```

**Advantages**

- stronger resource isolation;
- clearer multi-tenant boundary;
- reduces information leakage.

**Risks**

- slightly more architectural machinery;
- requires well-defined policy scopes.

## Current Project Position

The project uses policy scopes to establish ownership and visibility boundaries.

The intent is to prevent unauthorized resources from entering the application workflow.

## Engineering Lesson

Authorization is not only an action check.

It can also be a data-access boundary.

---

# 7. Abstraction vs Duplication

## The Question

When similar code appears in multiple places, should it immediately be extracted into an abstraction?

### Abstraction

**Advantages**

- reduces duplication;
- centralizes behavior;
- can establish a useful boundary.

**Risks**

- premature abstraction;
- hidden coupling;
- increased cognitive load;
- harder-to-understand APIs.

### Duplication

**Advantages**

- simple;
- local;
- easy to understand;
- allows patterns to emerge naturally.

**Risks**

- divergence;
- repeated bug fixes;
- inconsistent behavior.

## Current Project Position

The project prefers demonstrated need over speculative abstraction.

A small amount of duplication may be preferable to an abstraction whose purpose is unclear.

## Engineering Lesson

Duplication is sometimes cheaper than the wrong abstraction.

---

# 8. Explicit API Contract vs Ad-Hoc Responses

## The Question

Should every controller decide its own response shape?

### Ad-Hoc Responses

Each endpoint constructs responses independently.

**Advantages**

- maximum local flexibility;
- minimal initial design work.

**Risks**

- inconsistent clients;
- duplicated conventions;
- difficult API evolution;
- unclear failure semantics.

### Explicit Contract

The API establishes consistent success and failure structures.

```json
{
  "data": {},
  "meta": {}
}
```

and:

```json
{
  "error": {
    "code": "machine_readable_code",
    "message": "human_readable_message"
  }
}
```

**Advantages**

- predictable client behavior;
- explicit semantics;
- easier testing;
- easier documentation.

**Risks**

- requires discipline;
- changes become contract decisions.

## Current Project Position

The project uses an explicit API response contract documented in ADR-0001.

## Engineering Lesson

An API contract is a product boundary, not merely a serialization preference.

---

# 9. Synchronous vs Asynchronous Processing

## The Question

Should work execute during the request or through a background process?

### Synchronous

```text
Request
   ↓
Operation
   ↓
Response
```

**Advantages**

- simple;
- immediate feedback;
- easier consistency model.

**Risks**

- increases request latency;
- vulnerable to slow dependencies;
- unsuitable for long-running work.

### Asynchronous

```text
Request
   ↓
Enqueue
   ↓
Response

Worker
   ↓
Process
```

**Advantages**

- faster requests;
- better isolation of long-running work;
- enables retries.

**Risks**

- eventual consistency;
- retries and duplicates;
- operational complexity;
- more difficult debugging.

## Current Project Position

The project should introduce background processing when real workload characteristics justify it.

The existence of Sidekiq on the roadmap is not, by itself, sufficient reason to make every operation asynchronous.

## Engineering Lesson

Asynchronous processing solves latency and workload problems but introduces a distributed failure model.

---

# 10. Simple Architecture vs Advanced Architecture

## The Question

When should the project introduce patterns such as:

- Hexagonal Architecture;
- CQRS;
- Event-Driven Architecture;
- Saga;
- Outbox;
- Domain-Driven Design?

### Simple Architecture

**Advantages**

- easier to understand;
- faster to change;
- lower operational cost;
- fewer failure modes.

**Risks**

- may eventually become difficult to scale;
- boundaries may become insufficient as complexity grows.

### Advanced Architecture

**Advantages**

- can handle specific forms of complexity;
- stronger separation of concerns;
- can improve scalability or autonomy.

**Risks**

- cognitive overhead;
- operational complexity;
- more testing requirements;
- eventual consistency;
- harder debugging.

## Current Project Position

The project intentionally follows evolutionary architecture.

Advanced patterns should be introduced when the system creates architectural pressure that justifies them.

## Engineering Lesson

Architectural sophistication should be a response to complexity, not a substitute for understanding it.

---

# 11. Framework Convention vs Explicit Architecture

## The Question

How much should the architecture rely on Rails conventions versus explicit architectural boundaries?

### Framework-Driven Approach

Use Rails conventions extensively.

**Advantages**

- productive;
- familiar;
- less boilerplate;
- strong ecosystem support.

**Risks**

- framework coupling;
- implicit behavior;
- unclear boundaries as complexity grows.

### Explicit Architecture

Introduce explicit boundaries around:

- application services;
- policies;
- queries;
- serializers;
- errors;
- integrations.

**Advantages**

- clearer responsibilities;
- improved testability;
- easier reasoning about change.

**Risks**

- more code;
- possible over-engineering;
- fighting framework conventions.

## Current Project Position

The project follows a pragmatic middle ground.

Rails conventions remain valuable.

Explicit architecture is introduced where it improves understanding or protects important boundaries.

## Engineering Lesson

The goal is not to escape the framework.

The goal is to control where framework behavior becomes an architectural dependency.

---

# 12. AI Assistance vs Engineering Ownership

## The Question

How much responsibility should be delegated to AI tools?

### AI-Heavy Approach

AI generates:

- implementation;
- tests;
- documentation;
- architecture.

**Advantages**

- high velocity;
- exploration;
- reduced repetitive work.

**Risks**

- unexamined assumptions;
- architectural drift;
- hallucinated behavior;
- loss of engineering understanding.

### Engineer-Led AI

The engineer remains responsible for:

- problem definition;
- architectural decisions;
- validation;
- review;
- acceptance.

AI provides leverage rather than ownership.

## Current Project Position

The project explicitly follows:

```text
Engineer
    +
AI
    ↓
Engineering leverage
```

The AI must not become the source of truth.

The Git repository remains the durable source of project knowledge.

## Engineering Lesson

The important question is not:

> "Can AI write this?"

It is:

> "Can the engineer understand, evaluate, defend, and operate what AI helped produce?"

---

# 13. Documentation vs Implementation Speed

## The Question

How much documentation is justified during rapid development?

### Minimal Documentation

**Advantages**

- faster implementation;
- less maintenance.

**Risks**

- decisions become implicit;
- context is lost;
- future engineers repeat old discussions.

### Explicit Documentation

**Advantages**

- preserves reasoning;
- improves onboarding;
- supports AI agents;
- makes architectural evolution visible.

**Risks**

- documentation can become stale;
- excessive documentation creates maintenance overhead.

## Current Project Position

Document decisions that have meaningful architectural consequences.

Do not document every implementation detail.

The project treats ADRs and engineering memory as durable reasoning, not paperwork.

## Engineering Lesson

Documentation should preserve decisions and context that would otherwise be expensive to reconstruct.

---

# 14. Consistency vs Availability

## The Question

When systems become distributed, should operations prioritize immediate consistency or availability and eventual convergence?

### Strong Consistency

**Advantages**

- easier reasoning;
- immediate visibility of changes;
- simpler business semantics.

**Risks**

- coordination cost;
- reduced availability in some distributed scenarios;
- increased latency.

### Eventual Consistency

**Advantages**

- decoupled components;
- better scalability in some architectures;
- reduced coordination.

**Risks**

- stale data;
- more complex user experience;
- reconciliation requirements;
- harder testing.

## Current Project Position

The current application is not yet operating as a distributed event-driven system.

This trade-off becomes increasingly relevant if the architecture evolves toward:

- domain events;
- asynchronous processing;
- event-driven architecture;
- CQRS.

## Engineering Lesson

Eventual consistency should be accepted deliberately because the business can tolerate it, not merely because the architecture makes it convenient.

---

# 15. Build vs Buy

## The Question

Should the system implement infrastructure capabilities itself or rely on established external services?

Examples may include:

- authentication;
- observability;
- messaging;
- storage;
- payment infrastructure.

### Build

**Advantages**

- maximum control;
- customized behavior;
- potentially lower dependency on vendors.

**Risks**

- development cost;
- operational responsibility;
- maintenance burden;
- security responsibility.

### Buy / Adopt

**Advantages**

- faster delivery;
- mature infrastructure;
- reduced operational burden.

**Risks**

- vendor dependency;
- recurring cost;
- integration constraints;
- migration complexity.

## Current Project Position

The project should prioritize engineering learning where appropriate, but production-oriented decisions should consider whether building infrastructure actually creates product value.

## Engineering Lesson

Building something yourself is not automatically more technically impressive.

The correct decision depends on product value, risk, cost, and learning objectives.

---

# 16. Local Complexity vs Distributed Complexity

## The Question

Should a problem be solved inside one application/process or distributed across multiple components?

### Local

**Advantages**

- simpler deployment;
- simpler debugging;
- stronger consistency;
- easier testing.

**Risks**

- larger application boundaries;
- potential scaling limitations;
- tighter internal coupling.

### Distributed

**Advantages**

- independent scaling;
- component autonomy;
- isolation of workloads.

**Risks**

- network failures;
- eventual consistency;
- observability requirements;
- deployment complexity;
- operational overhead.

## Current Project Position

The project should prefer local solutions until distributed complexity solves a demonstrated problem.

## Engineering Lesson

Distribution is an architectural cost.

It should be introduced because the problem requires it, not because distributed systems are architecturally interesting.

---

# Using Trade-offs in Practice

A trade-off should be evaluated using the actual problem rather than a generic rule.

A useful process is:

```text
Problem
   ↓
Constraints
   ↓
Options
   ↓
Benefits
   ↓
Costs
   ↓
Risks
   ↓
Operational consequences
   ↓
Decision
   ↓
Evidence
   ↓
Reevaluation
```

The purpose of this document is to preserve that reasoning.

A good engineer is not someone who always chooses the same option.

A good engineer understands:

- why the choice was made;
- what was sacrificed;
- what assumptions support the decision;
- what evidence would invalidate it;
- when the opposite choice becomes more appropriate.
