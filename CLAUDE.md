# CLAUDE.md — Claude Code Instructions

This file gives Claude Code–specific instructions for working in this repository. It does not duplicate the project memory itself — see [`AGENTS.md`](AGENTS.md) for the general AI-agent contract that applies regardless of tool.

## Before doing meaningful work

Read, in this order:

1. [`AGENTS.md`](AGENTS.md) — the agent contract for this repository.
2. [`project/current-state.md`](project/current-state.md) — what is currently true.
3. [`project/roadmap.md`](project/roadmap.md) — where things are headed.
4. [`architecture/principles.md`](architecture/principles.md) — stable architectural principles.
5. Any ADRs in [`architecture/adr/`](architecture/adr/) relevant to the topic at hand.
6. Any session records in [`sessions/`](sessions/) relevant to the topic at hand — check [`journey/progress.md`](journey/progress.md) first to find which ones are relevant.

Do not skip this because the task looks small. A change that looks small in isolation (e.g. "add a note about X") can contradict a decision already recorded — that's exactly what this reading step is meant to catch.

## While working

Follow the rules in [`AGENTS.md`](AGENTS.md): don't invent history, preserve existing decisions unless asked to revisit them, keep changes small and reviewable, and be explicit about what's a fact vs. a decision vs. a proposal.

## After completing meaningful work

Update the memory repository to reflect it:

- If an architectural decision was made or confirmed, add an ADR under `architecture/adr/`, numbered sequentially.
- If the system's actual state changed (a feature shipped, a milestone completed, a plan changed), update `project/current-state.md`.
- If a reusable technical lesson came out of the work, add it under `journey/lessons/`.
- If the work corresponds to a discrete session, record it under `sessions/` (objective, context, work performed, decisions, lessons, open questions, next steps).
- Reflect any of the above in `journey/progress.md` where relevant.

Do not commit automatically — propose the changes and let the user review and commit, unless they've explicitly asked you to commit as part of the task. Use Conventional Commits when you do.
