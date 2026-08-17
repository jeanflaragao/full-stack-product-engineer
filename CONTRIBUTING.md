# Contributing

This repository is primarily maintained by a single author (Jean Aragão), working alongside AI agents (Claude Code, ChatGPT, Codex, and others). This document describes the workflow those contributions follow, whether human- or AI-authored.

## Workflow

```
Session
  ↓
Implementation / discussion (in betting-platform, or elsewhere)
  ↓
Architectural decision
  ↓
ADR (architecture/adr/)
  ↓
current-state.md update
  ↓
Lesson extraction (journey/lessons/)
  ↓
Progress update (journey/progress.md)
  ↓
Git commit
```

History is never reconstructed by guessing. If a step in this chain didn't happen, it isn't recorded.

## Commit conventions

Use [Conventional Commits](https://www.conventionalcommits.org/), e.g.:

```
docs: add project vision
docs: document application service failure contract
docs: update current project state
docs: record day 36 retrospective
```

Prefer small commits with a single, clear purpose over large batched ones.

## Rules that apply to any contributor (human or AI)

- Do not invent historical decisions, lessons, sessions, or progress. Mark unknowns explicitly as `TODO`.
- Do not rewrite historical session records under `sessions/`. Corrections go in a new entry, not a silent edit.
- Preserve existing architectural decisions (accepted ADRs) unless the change explicitly revisits them.
- Do not add application source code to this repository — that belongs in [`betting-platform`](https://github.com/jeanflaragao/betting-platform).

See [`AGENTS.md`](AGENTS.md) for the full AI-agent contract.
