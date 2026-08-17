# Project Glossary

## Purpose

This glossary defines terms with project-specific meaning across the Full Stack Product Engineer journey and the `betting-platform` application.

It is descriptive, not aspirational. When a term refers to planned rather than implemented capability, that distinction is explicit.

## Repository and Journey Terms

### Application Repository

The `betting-platform` repository. It contains the product implementation, tests, infrastructure, deployment configuration, and application-specific documentation. Its `ROADMAP.md` is authoritative for application implementation sequencing.

### Engineering Memory Repository

This repository, `full-stack-product-engineer`. It preserves project vision, historical foundation, current state, journey-level roadmap, architectural reasoning, lessons, and session history. It does not contain application source code.

### Foundation

The original goals, scope, technical direction, and learning objectives established at the beginning of the journey. [`foundation.md`](foundation.md) is historical source material and is not rewritten to match later implementation changes.

### Current State

The evidence-based account of what is actually true about `betting-platform` now. [`current-state.md`](current-state.md) distinguishes implemented, partially implemented, planned, and actively changing capabilities.

### Journey Roadmap

The capability-based learning and architectural-maturity progression recorded in [`roadmap.md`](roadmap.md). It is not the application implementation roadmap.

### Milestone

A meaningful stage in the engineering journey. Planned milestones belong in the roadmap; [`../journey/milestones.md`](../journey/milestones.md) records milestones only after repository evidence shows they were reached.

### Architecture Decision Record (ADR)

An immutable record of a meaningful architectural decision, including its context, alternatives, consequences, and trade-offs. Application ADRs live in `betting-platform/docs/adr/`; memory-repository ADRs are scoped by [`../architecture/adr/README.md`](../architecture/adr/README.md).

## Application and Domain Terms

### User

An authenticated application identity. In the current implementation, a user owns bookmakers.

### Bookmaker

A sportsbook operator represented as a resource in the application. The current implementation supports creating, listing, showing, and deleting bookmakers owned by a user.

### Account

A planned funded account associated with a bookmaker. No `Account` model or database table currently exists; an empty service stub is not implementation evidence.

### Betting Operations Management Platform

The product direction for `betting-platform`: an operational and analytical system for sportsbook accounts, financial activity, betting activity, expenses, profitability, and related workflows. The current implementation covers only an early subset of this domain.

### Bankroll

A planned capability for understanding money available across betting operations. It is part of the product direction and is not currently implemented.

### Financial Engine

The planned group of account and money-management capabilities. It has not started in the current schema.

### Resource Ownership

The relationship that determines which user owns a resource. Current bookmaker authorization and visibility behavior is based on ownership.

### Resource Visibility

Whether a caller is allowed to discover that a resource exists. The project intentionally treats visibility as a security concern and may return `404 Not Found` when another user's resource should not be disclosed.

## Architecture Terms

### Application Boundary

The point where internal application behavior is translated into an external protocol contract. In the Rails API, controllers and centralized error handling participate in this boundary.

### Application Error

A typed exception representing a known application or business failure. `ApplicationError` is the current base type for this error family. Unexpected programming or infrastructure failures are not automatically application errors.

### API Response Contract

The documented external response structure: successful responses use `data` and optional `meta`; failures use an `error` object with machine-readable `code` and human-readable `message`. The application decision is recorded in `betting-platform/docs/adr/0001-api-response-contract.md`.

### Error Boundary

The centralized mechanism that translates known internal exceptions into the API response contract. This architecture is partially implemented and currently being refined.

### Policy Object

A Pundit object that expresses authorization rules for actions on a resource. The current application uses `ApplicationPolicy` and `BookmakerPolicy`.

### Policy Scope

A Pundit scope that limits a collection to records visible to the caller. In this project it participates in ownership and information-leakage boundaries.

### Query Object

An object that isolates and composes read-side behavior. The bookmaker index currently composes filtering, searching, and sorting through query objects.

### Serializer

An explicit boundary between internal application objects and their API representation. The current bookmaker API uses `BookmakerSerializer`.

### Service Object / Application Service

A module-namespaced object representing a meaningful application operation. Current services use a class-level `.call(...)` convention that delegates to an instance `#call` method.

### Failure Propagation

The proposed service rule that a service allows a collaborator's known exception to move toward the application boundary unless handling that failure is explicitly part of the service's own responsibility. This is recorded as Proposed in `betting-platform/docs/adr/0002-application-service-failure-contract.md`.

## Status Terms

### Implemented

Evidence exists in application code and/or tests that the capability currently exists.

### Partially Implemented

Some implementation evidence exists, but the capability or its integration is incomplete.

### Planned

The capability appears in an accepted roadmap or design direction but lacks current implementation evidence.

### Current Work

The capability is actively being implemented or refactored and may include uncommitted application changes.

### Unknown

The available repositories and documentation do not provide enough evidence to determine the state confidently.
