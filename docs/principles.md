# Principles

Assumption Forge is based on a simple belief: the best tests protect against wrong assumptions, not just uncovered lines.

## 1. Risk before coverage

Coverage tells you which code ran. It does not tell you whether the right behavior was verified.

A high-coverage suite can still miss:

- invalid states,
- boundary values,
- duplicate data,
- partial failure,
- stale data,
- access mistakes,
- ordering issues,
- rounding errors,
- migration compatibility,
- concurrency problems.

Use coverage as a map, not as the destination.

## 2. Assumptions before assertions

Before writing an assertion, name the assumption it protects.

Weak:

```txt
assert result is not None
```

Stronger:

```txt
Assumption: duplicate ids are rejected instead of silently overwritten.
Assertion: result.error.code == "duplicate_id"
```

A test without a named assumption often becomes a snapshot of the current implementation.

## 3. Behavior before implementation

Prefer testing what callers can observe:

- returned values,
- persisted state,
- emitted events,
- error types,
- status codes,
- side effects,
- externally visible invariants.

Implementation details change. Behavior contracts should be harder to break accidentally.

## 4. Smallest reliable test

Use the smallest test that can expose the risk.

A unit test is not better because it is small. An end-to-end test is not better because it is realistic.

The right test is the smallest one that gives honest confidence.

## 5. No fake confidence

Fake confidence appears when a test passes but would not catch a meaningful bug.

Common causes:

- broad truthiness assertions,
- huge snapshots,
- copying production logic into expected values,
- excessive mocking,
- testing private implementation details without reason,
- checking only that no exception was thrown,
- using fixtures so complex nobody knows what matters.

Assumption Forge treats fake confidence as a test smell.

## 6. Explain the failure mode

Every valuable test should have a sentence like:

```txt
This test fails if <specific wrong behavior> happens.
```

If you cannot explain how a test fails usefully, the test probably needs work.

## 7. Tests should age well

Good tests survive refactors because they assert behavior, not internal shape.

When possible:

- assert stable contracts,
- keep fixtures small,
- make setup explicit,
- prefer named cases,
- isolate one risk per test,
- avoid asserting incidental ordering or formatting unless it is part of the contract.

## 8. Human uncertainty should be visible

AI agents should not invent product requirements silently.

When behavior is unclear, say:

```txt
Unclear: Should duplicate IDs be rejected or merged?
Assumption used for tests: Reject duplicates to avoid silent data loss.
```

This makes the test reviewable instead of pretending the requirement was obvious.
