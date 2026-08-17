# Full Stack Product Engineer

> A long-term engineering journey focused on becoming a production-grade Full Stack Product Engineer through deliberate practice, real systems, architectural decision-making, and continuous technical evolution.

## Purpose

This repository is the persistent engineering memory of the **Full Stack Product Engineer** journey.

It exists to preserve the knowledge, decisions, lessons, architectural reasoning, progress, and engineering practices developed throughout the project.

The goal is not simply to build software.

The goal is to become a stronger engineer by building, operating, reviewing, explaining, and continuously evolving a real software system.

This repository is intentionally separate from the application source code.

---

## Repository Relationship

The project is divided into two repositories with different responsibilities.

### Engineering Memory

This repository:

```text
full-stack-product-engineer
```

contains:

- project vision
- project foundation
- roadmap
- current state
- architecture principles
- Architecture Decision Records
- engineering lessons
- session history
- retrospectives
- mentorship guidelines
- AI-agent instructions
- engineering workflows

### Application

The actual product implementation lives in:

```text
betting-platform
```

Repository: [https://github.com/jeanflaragao/betting-platform](https://github.com/jeanflaragao/betting-platform)

The application repository contains the actual:

- backend
- frontend
- infrastructure
- tests
- configuration
- application code

The two repositories should evolve together but remain independently versioned.

## Core Principle

Git is the source of truth.

The project must not depend on the memory of a specific AI model.

ChatGPT, Claude Code, Codex, or any future AI agent may participate in the project, but the durable project knowledge must live in version-controlled files.

An agent should be able to clone this repository and understand:

- what the project is;
- why the project exists;
- what has already been decided;
- what is currently true;
- what has been learned;
- what remains to be done;
- how the project should be evolved.

## What This Project Is

The project is a deliberate engineering laboratory built around a real-world betting operations and analytics platform.

The application is designed to progressively evolve from a relatively simple Rails API into a production-grade SaaS platform.

The system explores areas including:

- sportsbook account operations
- betting operations
- financial operations
- profitability
- analytics
- account lifecycle
- operational intelligence
- SaaS architecture
- event-driven architecture
- observability
- background processing
- scalability
- production operations

The application is not intended to be merely a CRUD project.

It is intended to demonstrate professional engineering practices and architectural maturity.

## Engineering Objectives

The journey is designed to develop practical capability in:

- Ruby
- Ruby on Rails
- PostgreSQL
- Redis
- Sidekiq
- React
- TypeScript
- Docker
- CI/CD
- AWS
- automated testing
- observability
- system design
- SaaS architecture
- event-driven architecture
- product engineering
- technical communication

The project progressively introduces more advanced engineering concepts as the system evolves.

## Engineering Philosophy

The project prioritizes:

- architecture before implementation
- explicit trade-offs
- maintainability
- simplicity where appropriate
- production thinking
- automated testing
- security
- observability
- operational readiness
- evolutionary architecture
- documentation of important decisions

The objective is not to apply patterns for their own sake.

Every architectural technique should exist because it solves a real problem or creates a deliberate learning opportunity.

## Architecture Decision Records

Important architectural decisions are documented using Architecture Decision Records (ADRs).

ADRs capture:

- context
- problem
- decision
- consequences
- alternatives
- trade-offs

The purpose of an ADR is not merely to document what was implemented.

It should explain why the system evolved in a particular direction.

Examples of architectural areas already explored include:

- API response contracts
- exception architecture
- application service failure contracts
- controller responsibilities
- resource visibility
- authorization behavior
- information leakage

See: [`architecture/adr/`](architecture/adr/)

## Engineering Journey

The project is also a structured learning journey.

Each meaningful engineering session should preserve:

- objective
- context
- work performed
- architectural reasoning
- decisions
- lessons learned
- unresolved questions
- next steps

Historical session records are part of the project's permanent memory.

They should not be silently rewritten to make the history appear cleaner than it actually was.

The evolution of understanding is itself valuable engineering evidence.

## Mentorship Model

The project uses a Staff Engineer mentorship model.

The AI mentor is not primarily a code generator.

The mentor should help develop engineering judgment by:

- challenging assumptions
- identifying risks
- questioning architectural decisions
- explaining trade-offs
- reviewing designs
- reviewing implementation
- reviewing tests
- considering security
- considering observability
- considering operational concerns
- encouraging production thinking

The objective is to progressively develop the ability to independently make and defend engineering decisions.

## AI-Assisted Engineering

AI is an integral part of the engineering workflow.

However, AI must not become the project's source of truth.

Agents should:

- read project context before making significant changes;
- understand existing architectural decisions;
- avoid contradicting documented decisions without discussion;
- distinguish facts from assumptions;
- document meaningful new decisions;
- update project state when the system materially changes;
- preserve historical records;
- prefer small, reviewable changes.

The project should remain understandable and maintainable even if the AI model or agent changes.

See: [`AGENTS.md`](AGENTS.md) · [`CLAUDE.md`](CLAUDE.md) · [`ai/`](ai/)

## Repository Structure

```text
.
├── README.md
├── AGENTS.md
├── CLAUDE.md
│
├── project/
│   ├── vision.md
│   ├── foundation.md
│   ├── roadmap.md
│   ├── current-state.md
│   └── glossary.md
│
├── architecture/
│   ├── principles.md
│   ├── patterns.md
│   ├── tradeoffs.md
│   └── adr/
│
├── journey/
│   ├── progress.md
│   ├── milestones.md
│   ├── lessons/
│   └── retrospectives/
│
├── sessions/
│
├── mentorship/
│   ├── mentor-contract.md
│   └── review-rubric.md
│
└── ai/
    ├── agent-rules.md
    ├── prompts/
    └── workflows/
```

## Current Application

The application repository is:

**Betting Operations Management Platform**

[https://github.com/jeanflaragao/betting-platform](https://github.com/jeanflaragao/betting-platform)

The application is progressively evolving through multiple engineering milestones.

The current focus is application architecture and engineering maturity.

Future areas include:

- transactions
- background jobs
- idempotency
- logging
- observability
- domain events
- event-driven architecture
- outbox patterns
- CQRS
- AI-assisted engineering workflows

The exact current state is maintained in: [`project/current-state.md`](project/current-state.md)

## Development Workflow

The project follows professional engineering practices including:

- trunk-based development
- conventional commits
- pull requests
- automated testing
- architecture documentation
- code review
- changelogs
- incremental delivery

Changes to the application and changes to the engineering memory should remain traceable through Git history.

## Memory Lifecycle

The intended knowledge flow is:

```text
Engineering Session
        │
        ▼
Implementation / Investigation
        │
        ▼
Discussion & Reasoning
        │
        ▼
Architectural Decision
        │
        ├───────────────┐
        ▼               ▼
       ADR          Lesson Learned
        │               │
        └───────┬───────┘
                ▼
          Current State
                │
                ▼
             Progress
                │
                ▼
               Git
```

This creates a durable and auditable engineering history.

## Long-Term Goal

The ultimate goal of this project is to demonstrate the ability to:

- design production-grade systems
- build modern full stack applications
- reason about architecture
- evaluate trade-offs
- write maintainable software
- build reliable automated tests
- operate production systems
- evolve systems safely
- communicate technical decisions clearly
- work effectively with AI engineering tools
- demonstrate senior-level engineering judgment

The application is the implementation.

The engineering memory is the reasoning.

The journey is the evidence.
