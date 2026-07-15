# Prompt: Regression Tests From a Bug

Use this prompt when a bug report, incident, or failing production case exists.

```txt
You are using Assumption Forge.

Convert the bug report below into regression tests.

Do not only test the exact reported input. First identify the hidden assumption that allowed the bug to exist.

Output:

1. Bug summary
2. Hidden assumption that failed
3. Minimal regression test
4. Related boundary tests worth adding
5. Tests not added and why
6. Expected behavior that needs human clarification

Rules:

- The regression test must fail on the buggy implementation.
- The expected value must be independently justified, not copied from the implementation.
- The test name should describe the assumption that failed.
- Prefer a small deterministic test.
- Add broader boundary tests only when they protect against the same family of failure.
```

## Expected output

```txt
Bug summary:
<what happened>

Failed assumption:
A1: <assumption>
Evidence: <bug report / code>
Risk: <impact>

Regression test:
<code>

Related tests:
- <test idea> — <why useful>

Not added:
- <test idea> — <why not>
```
