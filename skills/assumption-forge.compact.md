# Assumption Forge — Compact Agent Skill

Use this when context is limited.

## Mission

Write tests from hidden assumptions, not from line coverage.

Before writing tests, ask:

> What must be true for this code to work, even though nobody wrote it down?

## Required process

1. Read the unit under test and nearby call sites.
2. Inspect existing tests and match project style.
3. Extract hidden assumptions.
4. Rank assumptions by risk.
5. Create a short test plan.
6. Write the smallest reliable tests.
7. Self-review for fake confidence.

## Assumption format

```txt
Assumption A1: <unstated belief>
Evidence: <code, type, call site, or existing behavior>
Risk: <what breaks if false>
Test idea: <how to challenge it>
Priority: High | Medium | Low
```

## Prioritize

High priority:

- access-control decisions,
- data integrity,
- money or billing,
- irreversible side effects,
- concurrency and idempotency,
- persisted state,
- silent failure.

Medium priority:

- invalid output,
- confusing errors,
- compatibility,
- ordering,
- pagination,
- configuration.

Low priority:

- already-covered behavior,
- low-impact cases,
- unrealistic inputs.

## Test quality rules

A good test:

- protects against one named assumption,
- has a clear failure mode,
- uses specific assertions,
- follows existing style,
- avoids excessive mocking,
- is deterministic,
- would fail if the implementation were subtly wrong.

A weak test:

- only increases coverage,
- mirrors the implementation,
- asserts only truthiness,
- snapshots too much without intent,
- mocks away the risk,
- tests private internals without a reason.

## Output format

```txt
Unit under test:

Hidden assumptions:

Test plan:

Test code:

Why these tests matter:

Gaps / human clarification:

Self-review:
```

## Golden rule

Passing tests do not prove the tests are valuable. Valuable tests protect against expectations nobody wrote down.
