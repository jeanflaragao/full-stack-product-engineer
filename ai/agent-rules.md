# AI Agent Rules

Repository-operational rules for AI agents working in this project. This file is a supplement to [`AGENTS.md`](../AGENTS.md), which is the canonical contract — read that first. This file covers day-to-day mechanics that follow from it.

## Referencing `betting-platform`

When a change here relates to something in `betting-platform`, reference it precisely: repository name, file path, commit SHA, PR number, or ADR number. Never copy application source code into this repository, even a short snippet, unless directly quoting it for the purpose of documenting a decision (and even then, prefer a reference plus a link/path over inlined code).

## Numbering conventions

- ADRs: `architecture/adr/0001-short-title.md`, sequential, never reused even if an ADR is later superseded.
- Sessions: `sessions/day-NNN.md`, sequential from the start of the journey (see `sessions/README.md` for how to determine the correct starting number).

## Distinguishing content types

When writing to this repository, be explicit about which of these a piece of content is:

- **Fact** — something true about the system today, verifiable by inspecting `betting-platform` or this repository.
- **Decision** — something chosen, with reasoning, that should be recorded as an ADR or in a session's decision log.
- **Assumption** — something taken as given but not verified.
- **Proposal** — something suggested but not yet decided.

Conflating these is the main way this repository would drift from being trustworthy.

## Reusable prompts and workflows

- [`ai/prompts/`](prompts/) — prompts used repeatedly during the journey, saved once they've proven useful more than once.
- [`ai/workflows/`](workflows/) — repeatable AI-assisted engineering workflows (multi-step procedures), saved once a workflow has been used enough times to be worth codifying.

Don't pre-populate either directory speculatively — add an entry when a prompt or workflow has actually proven reusable.
