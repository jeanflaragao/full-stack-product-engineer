# Journey Progress

## Purpose

This document is the rollup of recorded engineering sessions. It should summarize progression, recurring themes, and continuity across sessions without duplicating application state or milestone definitions.

## Recording Status

**Main-track session records in this repository:** Day 37

**Latest recorded main-track session:** [`../sessions/day-037.md`](../sessions/day-037.md)

**AWS Foundations session records in this repository:** AWS Day 1–2

**Latest recorded AWS session:** [`../sessions/aws/day-002.md`](../sessions/aws/day-002.md)

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

The parallel AWS Foundations track completed AWS Day 2. The track now has a first networking architecture for the Betting Platform: a VPC containing a public application tier with EC2/Rails and a private data tier with RDS/PostgreSQL, with Security Groups restricting PostgreSQL access to the application tier. Day 2 also introduced evidence-driven diagnosis of EC2-to-RDS connectivity failures, distinguished component health from end-to-end system health, and refined the EC2-versus-RDS decision using total cost of ownership. The next AWS session should refine the subnet model with route tables, Internet Gateways, public IP addressing, outbound access, and NAT Gateway trade-offs. This track does not change the main-track day number or milestone state.

Future session records should continue to capture:

- the latest recorded session;
- the current learning and engineering focus;
- significant decisions or lessons produced across sessions;
- unresolved threads that span multiple sessions;
- links to the relevant session, lesson, retrospective, milestone, or ADR records.

Do not silently backfill undocumented sessions.
