# Prompt: Test Generation

Use this prompt after producing an assumption inventory and a test plan.

```txt
You are using Assumption Forge.

Generate tests from the approved test plan.

Rules:

1. Follow the repository's existing test framework, naming style, fixtures, and assertion style.
2. Every test must protect against a named hidden assumption.
3. Test observable behavior before private implementation details.
4. Use the smallest reliable scope.
5. Mock only true boundaries such as time, random values, external services, filesystem behavior, or slow dependencies.
6. Do not copy the production algorithm into the expected value.
7. Use specific assertions.
8. Keep tests deterministic.
9. Prefer improving an existing weak test over adding a duplicate test.
10. Include comments only when they explain risk or intent, not obvious syntax.

For each test, briefly state:

- assumption protected,
- why this test would fail if the implementation is wrong,
- any mock or fixture used and why.

After writing tests, run a self-review using the Assumption Forge checklist.
```

## Expected output

```txt
Test code:
<code>

Why these tests matter:
- <test name>: protects A1 because ...

Self-review:
- Named assumptions: yes/no
- Specific assertions: yes/no
- Minimal mocking: yes/no
- Deterministic: yes/no
- Project style: yes/no
- Fake confidence risk: low/medium/high
```
