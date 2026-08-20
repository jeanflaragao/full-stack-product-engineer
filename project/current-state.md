# Current State

> Evidence-based snapshot of the `betting-platform` application.
>
> This document describes what is actually implemented, partially implemented, planned, and currently in progress. It should be updated when the application materially changes.

## Current Milestone

**Milestone 2 — Application Architecture**

Evidence includes:

- two ADRs in `docs/adr/`;
- ADR-0001 is Accepted;
- ADR-0002 is Proposed;
- Day 37 verification of the current bookmaker-destruction boundary;
- the verified Day 37 backend changes are pushed directly to `jeanflaragao/backend`'s `main` branch; no associated pull request was found;
- the root README identifies the project as being in the `core-domain-phase`.

---

# Implemented Capabilities

## Authentication

The application implements JWT authentication.

Current components include:

- JWT login;
- `Jwt::Encoder`;
- `Jwt::Decoder`;
- `Authenticatable` concern;
- `authenticate_user!` before-action enforcement.

---

## Authorization

Authorization is implemented using Pundit.

Current components include:

- `ApplicationPolicy`;
- `BookmakerPolicy`;
- ownership-based authorization;
- `policy_scope`.

Resource access is scoped through authorization policies.

---

## Bookmaker Management

The application currently supports:

- create bookmaker;
- list bookmakers;
- show bookmaker;
- destroy bookmaker.

The available routes expose these four operations.

There is currently no update action or route.

---

## Query Architecture

The bookmaker index uses a composed query pipeline:

```text
FilterQuery
    ↓
SearchQuery
    ↓
SortQuery
    ↓
Bookmakers::IndexQuery
```

The query architecture is used to compose read-side filtering, searching, and sorting behavior.

---

## Pagination

Pagination is implemented using Pagy.

Pagy is integrated into `ApplicationController` and used by the bookmaker index endpoint.

---

## Serialization

`BookmakerSerializer` is used for API serialization.

The serializer is used consistently for bookmaker responses.

---

## Testing

The application uses:

- RSpec;
- FactoryBot.

Existing tests include:

- authentication login request specs;
- bookmaker index request specs;
- bookmaker create request specs;
- bookmaker destroy request specs;
- `Bookmakers::DestroyService` service specs;
- policy specs;
- model specs;
- `Bookmakers::CreateService` service specs.

Day 37 strengthened deletion coverage across service and HTTP boundaries. The verified backend suite result is:

```text
30 examples, 0 failures
```

The service spec verifies deletion of the supplied bookmaker. Request specs verify `204 No Content` with an empty body and the intended database effect, plus state-preserving `404 Not Found` and `401 Unauthorized` failures. Invisible and nonexistent resources use the structured `resource_not_found` contract.

---

## Database

The current schema contains:

- `users`
- `bookmakers`

The current relationship is:

```text
User
  has_many :bookmakers

Bookmaker
  belongs_to :user
```

Bookmakers are configured with dependent destruction through the user relationship.

---

## Local Development

Local PostgreSQL development uses:

- PostgreSQL 16;
- Docker Compose.

The current Compose configuration provides PostgreSQL only.

There is currently no application container and no Redis container in the local Compose configuration.

---

## Runtime Versions

The Docker configuration specifies:

`Ruby 3.2.2`

The project uses:

`Rails 8.0.5`

There is currently an inconsistency because `.ruby-version` contains:

`system`

while the Dockerfile specifies Ruby 3.2.2.

---

# Partially Implemented Capabilities

## Exception and Error Architecture

The application has an application error boundary for translating known failures into the API response contract.

The intended architecture includes:

- `ApplicationError` as the application-level error base;
- typed application error subclasses;
- centralized error handling;
- `ErrorHandler`;
- translation of known exceptions into the API response contract.

Current implementation includes:

- `AuthenticationError`;
- `ValidationError`;
- centralized `rescue_from` handling;
- structured translation of authentication and resource-not-found failures.

ADR-0002 remains Proposed. The current application still lacks a real business workflow in which a known application exception propagates from a service to the HTTP boundary, so Day 37 does not provide the evidence required to accept that decision.

## Bookmaker Deletion Boundary

`Bookmakers::DestroyService` synchronously destroys the authorized bookmaker with `destroy!`.

The HTTP boundary returns:

- `204 No Content` with an empty body after successful deletion;
- `404 Not Found` when the resource is nonexistent or outside the authenticated user's visible scope;
- `401 Unauthorized` for an unauthenticated request.

There is no active-account deletion guard. The application has no `Account` model, `accounts` table, bookmaker-account association, or active-account lifecycle, so such a guard is not currently executable domain behavior. Day 37 removed `Bookmakers::ActiveAccountsExistError` under YAGNI; it should be reconsidered only when the Account domain supplies a real invariant.

---

## CI/CD

A CI workflow exists at:

`.github/workflows/ci.yml`

However, the workflow is currently completely commented out.

The root README confirms that CI is scaffolded but disabled.

Therefore, automated CI enforcement is currently not active.

---

## Deployment

Kamal deployment scaffolding exists in:

`config/deploy.yml`

The configuration is not production-ready.

It still contains placeholder configuration, including a placeholder IP and image configuration.

---

# Planned Capabilities

The following capabilities have roadmap evidence but no corresponding implementation evidence in the current application.

## Accounts and Financial Engine

The application roadmap includes account and financial functionality.

However, there is no `accounts` table or `Account` model. Day 37 removed the premature `Accounts::CreateService` placeholder because its path established an invalid Zeitwerk autoloading contract without executable domain behavior.

`Bookmakers::UpdateService` is also currently an empty stub.

---

## Frontend

The `frontend/` directory currently contains only a README.

There is no frontend application implementation yet.

---

## Observability

There is currently no evidence of:

- Sentry;
- Honeybadger;
- Lograge;
- APM tooling.

---

## Background Jobs

There is currently no evidence of:

- Sidekiq;
- real background job classes;
- implemented asynchronous workflows.

---

## Event-Driven Architecture

There is currently no implemented event-driven architecture.

The capability remains part of the planned engineering roadmap.

---

## Additional Planned Capabilities

There is currently no implementation evidence for:

- multi-tenancy;
- feature flags;
- audit logs;
- API versioning beyond `/v1`;
- caching.

---

# Current Architecture

The current application architecture can be summarized as:

```text
HTTP Request
     ↓
Controller
     ↓
Service
     ↓
Model
```

Read-side behavior additionally uses Query Objects:

```text
Controller
     ↓
Index Query
     ├── Filter Query
     ├── Search Query
     └── Sort Query
```

Authorization is handled through Pundit policy objects.

The service convention follows:

`Klass.call(...)`

which delegates to an instance-level:

`#call`

method.

---

## Current Error Boundary Architecture

The application uses the following boundary for implemented known failures:

```text
Service
   ↓
Raise typed application exception
   ↓
Application boundary
   ↓
ErrorHandler
   ↓
API error contract
```

The `rescue_from` ordering in the error handler is considered load-bearing for the current design.

The broader service-failure propagation contract remains proposed until a real business workflow validates it.

---

## API Response Contract

The current API contract is:

### Success

```json
{
  "data": {},
  "meta": {}
}
```

### Failure

```json
{
  "error": {
    "code": "machine_readable_code",
    "message": "human_readable_message"
  }
}
```

The current architectural decision also establishes:

- 404 Not Found for resources that are not visible to the caller;
- 403 Forbidden when the resource exists but the caller is not authorized;
- 204 No Content for successful deletion.

The authoritative decision is documented in ADR-0001.

---

# Architectural Decisions

## ADR-0001 — API Response Contract

**Status:** Accepted

Defines:

- successful response structure;
- structured failure responses;
- 404 versus 403 behavior;
- 204 behavior for DELETE operations.

Location:

`docs/adr/0001-api-response-contract.md`

---

## ADR-0002 — Application Service Failure Contract

**Status:** Proposed

Defines the principle that application services do not rescue exceptions raised by collaborating services unless the exception is explicitly part of their own business responsibility.

Known application exceptions should propagate until they reach an appropriate application boundary where they can be translated into the API contract.

Location:

`docs/adr/0002-application-service-failure-contract.md`

Day 37 clarified the intended boundary but removed the artificial active-account workflow. This ADR remains Proposed until a real domain workflow validates exception propagation from a service to the HTTP boundary.

---

# Repository Architecture

The `betting-platform` repository is currently structured as a repository shell containing infrastructure and documentation while the backend is separately versioned as a Git submodule.

The backend therefore has its own:

- Git history;
- CI configuration;
- README;
- Claude instructions.

This repository split is an existing architectural characteristic of the application project.

---

# Current Technical Debt

Known technical debt includes:

- `Bookmakers::UpdateService` is an empty stub.
- `.ruby-version` is inconsistent with the Dockerfile Ruby version.
- Query Objects do not currently have dedicated specs.
- There is no `User` model spec.
- CI is completely disabled.
- Kamal deployment configuration contains placeholders.
- ADR-0002 remains Proposed.
- The Account domain and its deletion invariant remain future product work.

---

# Latest Completed Work

Day 37 completed the design and verification of the current bookmaker-destruction boundary.

Completed work includes:

- removing premature Account-related production code;
- verifying synchronous authorized deletion;
- adding a `Bookmakers::DestroyService` service spec;
- strengthening success and failure request specs;
- verifying `30 examples, 0 failures`;
- verifying `git diff --check` with no whitespace errors;
- verifying Zeitwerk eager loading with `All is good!`.

GitHub shows the Day 37 changes pushed directly to `jeanflaragao/backend`'s `main` branch. No open or merged pull request was found for these commits.

---

# Recommended Next Engineering Step

The next logical product step is to design the Account domain when its requirements are ready. Only then should bookmaker deletion be revisited for a real active-account invariant.

ADR-0002 should remain Proposed until a genuine application exception propagation workflow validates it. Independently, re-enabling CI remains an important production-readiness step because the workflow exists but is disabled.

---

# Current Direction

The project is currently transitioning from:

```text
Rails API Foundations
        ↓
Application Architecture
```

The immediate focus is therefore not on introducing new infrastructure or distributed architecture.

The focus is on establishing reliable application boundaries, explicit failure contracts, testable business behavior, and clear architectural responsibilities.

The next milestone is Production Readiness, but the project should not move there until the current application architecture has been sufficiently established and validated.

---

# State Classification

The current state should be interpreted using the following categories:

| Status | Meaning |
|---|---|
| Implemented | Evidence exists in the application code and/or tests |
| Partially Implemented | Some implementation exists, but the capability is incomplete |
| Planned | Present in roadmap or design but without implementation evidence |
| Current Work | Actively being implemented or refactored |
| Unknown | Insufficient evidence to determine the state |

When this document is updated, these distinctions should be preserved.

---

# Maintenance Rule

This document describes the current state of the application, not the desired future state.

When a capability moves from:

```text
Planned
    ↓
Implemented
```

the current state should be updated.

When an architectural decision changes, the relevant ADR should be updated through a new decision rather than rewriting history.

When a significant milestone changes, the roadmap and journey documentation should also be evaluated.

The goal is for a new engineer or AI agent to be able to read this document and understand:

> "This is what actually exists right now."
