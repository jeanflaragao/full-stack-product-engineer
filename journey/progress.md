# Journey Progress

## Purpose

This document is the rollup of recorded engineering sessions. It should summarize progression, recurring themes, and continuity across sessions without duplicating application state or milestone definitions.

## Recording Status

**Main-track session records in this repository:** Day 37

**Latest recorded main-track session:** [`../sessions/day-037.md`](../sessions/day-037.md)

**AWS Foundations session records in this repository:** AWS Day 1

**Latest recorded AWS session:** [`../sessions/aws/day-001.md`](../sessions/aws/day-001.md)

The user explicitly identified the completed work as Day 37. Earlier numbered sessions remain unrecorded in this repository and have not been reconstructed.

No retrospective session history has been invented to fill this gap.

## Current Orientation

Use these sources for established context:

- [`../project/current-state.md`](../project/current-state.md) for what is actually true about the application now;
- [`milestones.md`](milestones.md) for milestones supported by evidence;
- [`../project/roadmap.md`](../project/roadmap.md) for the current journey-level direction;
- `betting-platform/ROADMAP.md` for authoritative application implementation sequencing;
- `betting-platform/SESSION.md` for tactical application context, with its documented staleness considered.

## Current Journey Position

The evidence-based documents currently place the journey at:

```text
Milestone 0 — Foundations
        ↓ completed
Milestone 1 — Rails API Foundations
        ↓ completed
Milestone 2 — Application Architecture
        ↓ in progress
Production Readiness
        ↓ planned
```

This orientation is derived from current state and milestone records, not from reconstructed session history.

## Current Learning Focus

The main track remains at Day 38 and Milestone 2 — Application Architecture. Day 37 established that future requirements should remain in roadmap and documentation until their domain dependencies exist. It verified synchronous bookmaker deletion, reinforced the distinction between service behavior and HTTP contracts, and kept ADR-0002 Proposed pending a real exception-propagation workflow.

The parallel AWS Foundations track completed AWS Day 1. It established the cloud and shared-responsibility mental models, compared EC2-hosted PostgreSQL with RDS, and introduced single points of failure and availability trade-offs. This track does not change the main-track day number or milestone state.

Future session records should continue to capture:

- the latest recorded session;
- the current learning and engineering focus;
- significant decisions or lessons produced across sessions;
- unresolved threads that span multiple sessions;
- links to the relevant session, lesson, retrospective, milestone, or ADR records.

Do not silently backfill undocumented sessions.
