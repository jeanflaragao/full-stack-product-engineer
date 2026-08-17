# Reached Milestones

## Purpose

This document records major journey milestones after repository evidence shows they were reached.

It is intentionally historical. Planned milestones and capability sequencing belong in [`../project/roadmap.md`](../project/roadmap.md); the application's implementation sequence belongs in `betting-platform/ROADMAP.md`.

Completion dates are marked unknown when they cannot be established confidently from the available history.

---

# Milestone 0 — Foundations

**Status:** Completed

**Completion date:** Unknown — not currently documented

## Outcome

The project established a reproducible development and repository foundation for the application and the engineering journey.

## Evidence

- Rails API project structure;
- PostgreSQL 16 available through Docker Compose;
- root repository plus separately versioned backend submodule;
- Git workflow using Conventional Commits and feature branches;
- repository documentation structure;
- basic CI and deployment scaffolding.

## Qualification

Completion means the foundation was established, not that every operational concern is production-ready. The current state still records disabled CI and placeholder deployment configuration as incomplete capabilities.

---

# Milestone 1 — Rails API Foundations

**Status:** Completed

**Completion date:** Unknown — not currently documented

## Outcome

The application established a functioning Rails API foundation around authenticated, user-owned bookmaker resources.

## Evidence

- JWT authentication;
- Pundit authorization and policy scopes;
- bookmaker create, index, show, and destroy operations;
- Pagy pagination;
- composed filtering, searching, and sorting query objects;
- explicit bookmaker serialization;
- RSpec and FactoryBot test infrastructure;
- request, policy, model, and service specs;
- structured API response and error-contract groundwork.

## Qualification

Completion does not imply full CRUD or a production-ready API. Bookmaker update is absent, the error architecture is still being consolidated, and CI is not active.

---

# Current Position — Milestone 2: Application Architecture

**Status:** In Progress — not yet a reached milestone

The current work is strengthening service boundaries, controller responsibilities, exception handling, API contracts, authorization behavior, and testing practices.

This section records orientation only. Completion should be added as a reached milestone after the criteria in [`../project/roadmap.md`](../project/roadmap.md) are supported by implementation and documentation evidence.
