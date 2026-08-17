# Review Rubric

A consistent framework for reviewing work in this journey, used by the mentor (see [`mentor-contract.md`](mentor-contract.md)). Not every dimension applies to every review — use judgment about which are load-bearing for the change at hand, but consider all of them.

## Architecture

Does this fit the existing layering and service boundaries, or does it quietly cross one? Does it belong in an existing component, or does it justify a new one? Is it consistent with accepted ADRs, or does it implicitly revisit one without saying so?

## Correctness

Does it do what it claims to do, including at the edges — concurrent access, partial failure, empty/null/boundary inputs? Is the happy path the only path that's been considered?

## Maintainability

Would a reader unfamiliar with this specific change understand it from the code and its tests? Is complexity proportional to the problem, or is there premature abstraction (or premature flatness)?

## Testing

Do the tests exercise the actual risk in the change, not just line coverage? Do they fail for the right reason when the implementation is wrong? Are edge cases and failure modes tested, not just the happy path?

## Security

What does this trust that it shouldn't? What does this expose (in responses, logs, error messages) that it shouldn't? Is authorization checked at the right boundary?

## Performance

Does this introduce an N+1, an unbounded query, or a lock held longer than necessary? Is the performance characteristic acceptable at the scale this system actually expects to operate at?

## Observability

If this fails in production, is there enough signal (logs, metrics, traces) to diagnose why without reproducing it locally?

## Operability

What does deploying this require? Is it safe to roll back? Does it need a migration, and is that migration safe to run against live data?

## Product thinking

Does this solve the actual problem, or a proxy for it? Is the scope right — not gold-plated, not under-scoped relative to what was asked?
