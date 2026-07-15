# Prompt: Test Plan

Use this prompt after extracting assumptions and before generating tests.

```txt
You are using Assumption Forge.

Given the unit under test and the hidden assumptions listed below, create a prioritized test plan.

Do not write test code yet.

For each planned test, include:

- test name,
- assumption protected,
- risk covered,
- test type,
- setup required,
- expected assertion,
- expected failure mode if the implementation is wrong.

Prefer the smallest reliable test. Do not choose an integration test when a unit test can expose the risk. Do not choose an end-to-end test unless the behavior cannot be verified at a smaller boundary.

Also list tests intentionally not added and explain why they are lower value, redundant, too broad, or require missing product clarification.
```

## Expected output

```txt
Test Plan

1. <test_name>
   Assumption: A1
   Risk: <risk>
   Type: Unit | Integration | Contract | Property | Regression | Fuzz | E2E
   Setup: <fixtures, mocks, data>
   Assertion: <specific assertion>
   Failure mode: <what wrong implementation this catches>

2. ...

Tests intentionally not added:
- <test idea> — <reason>

Required clarification:
- <only if needed>
```
