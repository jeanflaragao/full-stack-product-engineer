# Architecture Decision Records

An ADR is a short, immutable record of a single architectural decision: the context that forced it, the decision itself, and the trade-offs accepted.

## Scope

This directory is for ADRs that belong to this memory repository's own scope — decisions about the engineering journey, mentorship process, or cross-project architecture knowledge.

ADRs about `betting-platform`'s own application architecture are recorded in that repository, at `docs/adr/`. As of this scaffolding, `betting-platform` already has:

- `0001-api-response-contract.md` (Accepted)
- `0002-application-service-failure-contract.md` (Proposed)

Whether and how those get referenced or mirrored here (vs. left solely in `betting-platform`) is an open question — TODO: decide and document that policy before the first ADR is added to this directory.

## Format

Each ADR is a single Markdown file, numbered sequentially: `architecture/adr/0001-short-title.md`.

```markdown
# 0001: Short title of the decision

## Status
Proposed | Accepted | Superseded by 000X

## Context
What forced this decision? What constraints were in play?

## Decision
What was decided.

## Consequences
What becomes easier or harder as a result. What trade-offs were accepted.
```

## Index

_(empty — no ADRs recorded in this repository's own scope yet)_
