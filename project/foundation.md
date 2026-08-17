# Full Stack Product Engineer — Project Foundation

> Original foundation established at the beginning of the Full Stack Product Engineer journey.
>
> This document preserves the original goals, scope, technical direction, and learning objectives. It is historical project context and should not be rewritten merely because the implementation has evolved.

---

# Overview

This project is a long-term technical training program focused on becoming a strong Full Stack Product Engineer capable of building modern production-grade SaaS applications.

The project will simulate a real-world engineering environment and will be used as:

- a professional portfolio project
- a technical laboratory
- an interview preparation environment
- a system design learning platform
- a backend and frontend engineering training project
- a DevOps and cloud learning environment

The main application will evolve progressively from a simple CRUD system into a scalable SaaS platform with modern architecture patterns.

---

# Main Project

## Bet Analytics Platform

A SaaS platform for sports betting analytics and management.

The platform will allow users to:

- create and manage bets
- track profit and loss
- analyze ROI
- visualize performance metrics
- manage bookmakers and tipsters
- generate dashboards
- filter analytics data
- receive automatic insights
- monitor streaks
- analyze historical performance

---

# Main Technical Goals

This project is designed to develop practical experience with:

- Ruby
- Ruby on Rails API
- React
- TypeScript
- PostgreSQL
- Redis
- Background Jobs
- Event-Driven Architecture
- Docker
- CI/CD
- AWS
- Observability
- Testing
- System Design
- SaaS Architecture
- Product Engineering
- Technical English

---

# Engineering Philosophy

The goal is NOT to build only a CRUD application.

The goal is to build a production-grade SaaS platform that demonstrates:

- architecture decisions
- scalability thinking
- testing maturity
- product thinking
- software quality
- operational excellence
- maintainability
- modern engineering practices

---

# Monorepo Structure

```text
project-root/
  backend/
  frontend/
  infra/
  docs/
```

---

# Backend Stack

- Ruby
- Ruby on Rails API
- PostgreSQL
- Redis
- Sidekiq
- RSpec
- FactoryBot
- Rubocop
- JWT
- Pundit
- Kafka (advanced phase)

---

# Frontend Stack

- React
- TypeScript
- Vite
- React Query
- Zustand
- TailwindCSS
- Recharts

---

# Infrastructure Stack

- Docker
- Docker Compose
- GitHub Actions
- AWS
- Nginx

---

# Observability Stack

- Sentry
- Structured Logs
- Health Checks
- Metrics
- CloudWatch

---

# SaaS Multi-Tenant Architecture

The application will follow a multi-tenant SaaS architecture.

Instead of:

> User owns bets

the system will evolve to:

> Organization owns users and bets

## Multi-Tenant Model

```text
Organization
  has_many :users
  has_many :bets
  has_many :tipsters
  has_many :bookmakers
```

This architecture will teach:

- SaaS fundamentals
- data isolation
- authorization
- scalable architecture
- enterprise modeling
- security best practices

---

# Event-Driven Architecture

The application will progressively adopt event-driven patterns.

Instead of controllers performing multiple responsibilities directly:

- create bet
- update metrics
- update balance
- send notifications
- create audit logs

the system will evolve into:

> `BetCreated` event

Listeners will react independently:

- `UpdateAnalytics`
- `UpdateDailyBalance`
- `NotifyUsers`
- `CreateAuditLog`

## Why Event-Driven Architecture Matters

This approach teaches:

- decoupling
- scalability
- distributed systems thinking
- asynchronous processing
- modern backend architecture
- maintainability

The project will initially use:

`ActiveSupport::Notifications`

and progressively evolve toward more advanced event-driven approaches when justified.

---

# CI/CD Pipeline

The project must include a professional CI/CD pipeline.

Pipeline stages:

- run tests
- run lint
- build docker images
- deploy staging
- deploy production

---

# Development Workflow

The project will simulate a professional engineering environment.

The workflow must include:

- trunk-based development
- conventional commits
- pull requests
- changelogs
- self code review
- architecture documentation
- issue tracking

---

# Weekly Study Structure

## Monday

Rails backend

## Tuesday

React frontend

## Wednesday

Testing and architecture

## Thursday

SQL and performance

## Friday

CI/CD and AWS

## Saturday

Hands-on implementation

## Sunday

Review and technical English

---

# English Learning Integration

Every class MUST include a short English lesson.

Requirements:

- maximum duration: 15 minutes
- focused on technical English
- related to the current lesson topic
- focused on real engineering communication

## English Topics Examples

- explaining technical decisions
- backend vocabulary
- frontend vocabulary
- DevOps vocabulary
- API communication
- pull request communication
- interview communication
- debugging explanations
- architecture discussions

## English Practice Requirements

The project should progressively include:

- commits written in English
- pull request descriptions in English
- technical documentation in English
- feature explanations in English
- short spoken explanations in English

## English Class Trigger Rule

The English lesson should ONLY be generated when the user explicitly types the command:

`ENGLISH CLASS`

If the command is not provided, the session should focus only on the engineering and product development topics.

---

# Advanced Features

The project should progressively include:

- feature flags
- audit logs
- retry jobs
- rate limiting
- API versioning
- caching
- pagination
- observability
- background processing
- event-driven workflows
- multi-tenancy
- CSV import
- notifications
- webhooks

---

# Roadmap

## Month 1

- backend setup
- authentication
- CRUD bets
- PostgreSQL
- basic tests
- multi-tenancy foundations

## Month 2

- React dashboard
- charts
- filters
- frontend/backend integration

## Month 3

- advanced testing
- Docker
- CI/CD
- deployment

## Month 4

- AWS
- observability
- performance
- architecture improvements

## Month 5+

- advanced scalability
- event-driven architecture
- system design
- mock interviews
- advanced DevOps
- advanced backend patterns

---

# Final Expected Outcome

By the end of the project, the objective is to be able to:

- build modern SaaS applications
- explain architecture confidently
- discuss technical trade-offs
- design scalable systems
- write professional automated tests
- operate production systems
- work as a Full Stack Product Engineer
- succeed in international technical interviews
- present a strong engineering portfolio
