# Prompt: Self-Review Generated Tests

Use this prompt after an AI agent writes tests.

```txt
You are using Assumption Forge.

Review the generated tests before finalizing them.

For each test, answer:

1. Which hidden assumption does this test protect?
2. Would this test fail if the implementation were subtly wrong?
3. Is the assertion specific enough?
4. Does the test validate behavior instead of implementation details?
5. Are mocks minimal and honest?
6. Is the fixture readable?
7. Is the test deterministic?
8. Does the test match existing project style?
9. Is the test redundant with an existing test?
10. Does this test create fake confidence?

Then produce a final quality verdict:

- Keep as-is
- Improve before merging
- Remove as redundant
- Needs human product clarification

If a test is weak, rewrite it.
```

## Expected output

```txt
Self-review

Test: <name>
Assumption protected: A1
Failure value: High | Medium | Low
Specific assertions: Yes | No
Mock quality: Good | Too much | Missing boundary
Fake confidence risk: Low | Medium | High
Verdict: Keep | Improve | Remove | Clarify

Recommended changes:
- <change>
```
