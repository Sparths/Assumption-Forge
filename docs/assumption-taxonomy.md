# Assumption Taxonomy

Use this taxonomy to help AI agents find hidden assumptions systematically.

## Data shape assumptions

The code may assume:

- required fields exist,
- optional fields are handled,
- types match expectations,
- unknown fields are harmless,
- schema versions are compatible,
- nested data is complete,
- arrays are not empty,
- IDs are valid and unique.

Example test idea:

```txt
Provide missing optional fields and verify explicit fallback behavior.
```

## Value assumptions

The code may assume:

- numbers are finite,
- strings are normalized,
- dates are valid,
- currency has expected precision,
- percentages are bounded,
- enum values are known,
- paths are normalized,
- URLs have expected shape.

Example test idea:

```txt
Use zero, negative, very large, and non-finite numeric values where relevant.
```

## Ordering assumptions

The code may assume:

- database rows are ordered,
- map iteration is stable,
- events arrive in order,
- timestamps are monotonic,
- sorting ties are deterministic,
- pagination cursors are stable.

Example test idea:

```txt
Provide equal sort keys and verify deterministic tie handling.
```

## Time assumptions

The code may assume:

- timezone is UTC,
- daylight saving does not matter,
- clock does not jump,
- expiration boundaries are obvious,
- scheduled tasks run exactly once,
- retry windows are respected.

Example test idea:

```txt
Freeze time near midnight, month-end, year-end, leap day, or daylight-saving transitions.
```

## State assumptions

The code may assume:

- state transitions are valid,
- partial updates cannot happen,
- retries are idempotent,
- caches are fresh,
- locks are held,
- cleanup always runs,
- migrations completed successfully.

Example test idea:

```txt
Simulate a failure halfway through a workflow and assert persisted state remains consistent.
```

## Dependency assumptions

The code may assume:

- external services return complete data,
- APIs are available,
- network calls are fast,
- file writes succeed,
- database transactions commit,
- environment variables exist,
- feature flags have valid values.

Example test idea:

```txt
Make a dependency return incomplete data and verify the caller handles it explicitly.
```

## Access assumptions

The code may assume:

- identity implies permission,
- roles are current,
- tenant boundaries are enforced elsewhere,
- object ownership was already checked,
- feature access is enabled for the user,
- expired credentials cannot reach this code.

Example test idea:

```txt
Use a valid identity without the required permission and verify the operation is rejected.
```

## Compatibility assumptions

The code may assume:

- old data matches new code,
- clients and servers deploy together,
- deprecated fields are unused,
- platform behavior is identical,
- locale differences do not matter,
- filesystem case sensitivity does not matter.

Example test idea:

```txt
Load a payload from a previous schema version and verify migration or fallback behavior.
```

## Presentation assumptions

The code may assume:

- formatting is locale-independent,
- casing is stable,
- whitespace is irrelevant,
- messages are not parsed by machines,
- snapshots are stable,
- ordering in rendered output is deterministic.

Example test idea:

```txt
Verify output formatting for the exact public contract while avoiding incidental implementation details.
```
