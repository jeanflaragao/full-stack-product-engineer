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
- active uncommitted work in the backend submodule implementing the exception architecture described by those ADRs;
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
- policy specs;
- model specs;
- `Bookmakers::CreateService` service specs.

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

The application is currently undergoing an exception-handling refactor.

The intended architecture includes:

- `ApplicationError` as the application-level error base;
- typed application error subclasses;
- centralized error handling;
- `ErrorHandler`;
- translation of known exceptions into the API response contract.

Current work includes:

- `AuthenticationError`;
- `ValidationError`;
- centralized `rescue_from` handling;
- replacing ad-hoc JSON error rendering with raised application exceptions.

The work is currently uncommitted.

---

## Bookmaker Deletion Guard

`Bookmakers::ActiveAccountsExistError` exists but is not currently raised by the destruction flow.

The current `Bookmakers::DestroyService` calls:

`bookmaker.destroy!`

The business-rule guard therefore exists as a defined concept but is not yet wired into the service behavior.

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

However:

- `app/services/accounts/create_service.rb` is an empty stub;
- there is no `accounts` table;
- there is no `Account` model.

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

The application is currently transitioning toward:

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

This architecture is currently being implemented and validated through tests.

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

This ADR remains Proposed until the implementation and tests validate the approach.

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

- `ActiveAccountsExistError` exists but is not raised.
- `Bookmakers::UpdateService` is an empty stub.
- `Accounts::CreateService` is an empty stub.
- `.ruby-version` is inconsistent with the Dockerfile Ruby version.
- Query Objects do not currently have dedicated specs.
- There is no `User` model spec.
- CI is completely disabled.
- Kamal deployment configuration contains placeholders.
- The exception-handling refactor is currently uncommitted.
- ADR-0002 remains Proposed.
- The backend currently has uncommitted changes related to the error-handling refactor.

---

# Current Work in Progress

The active engineering work is centered on implementing the exception architecture defined by ADR-0001 and ADR-0002.

Current work includes:

- centralizing error handling in `ErrorHandler`;
- moving Pundit and Pagy rescue behavior into the centralized error boundary;
- replacing ad-hoc authentication error responses with `AuthenticationError`;
- introducing `ValidationError` for `RecordInvalid`;
- expanding request specs;
- testing the behavior where a bookmaker belongs to another user;
- validating the 404 resource-visibility behavior.

The backend currently contains:

- 9 modified files
- 2 untracked files

related to this work.

---

# Recommended Next Engineering Step

The immediate engineering priority is to finish and validate the ADR-0001/ADR-0002 implementation.

This includes:

- complete the centralized exception handling;
- complete the relevant request and service specs;
- wire `ActiveAccountsExistError` into `Bookmakers::DestroyService`;
- verify the resulting behavior;
- commit the implementation;
- promote ADR-0002 from Proposed to Accepted once validated by implementation and tests.

After that, the next major engineering priority is to re-enable CI because the workflow already exists but is currently disabled.

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
