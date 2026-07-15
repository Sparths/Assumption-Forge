# Prompt: Assumption Extraction

Use this prompt when you want an AI agent to inspect code before writing tests.

```txt
You are using Assumption Forge.

Analyze the code below or the referenced files. Do not write tests yet.

Your task is to identify hidden assumptions: things the code relies on but does not explicitly enforce, document, or test.

For each assumption, use this format:

Assumption A1: <unstated belief>
Evidence: <specific code, type, branch, call site, or naming evidence>
Risk: <what can go wrong if this is false>
Test idea: <how a test could challenge it>
Priority: High | Medium | Low
Recommended test type: Unit | Integration | Contract | Property | Regression | Fuzz | E2E

Look especially for assumptions about:

- missing, null, undefined, None, or empty values,
- malformed input,
- duplicate values,
- ordering,
- time and timezone behavior,
- money, rounding, precision, and numeric boundaries,
- permissions and access rules,
- retries and idempotency,
- partial failure,
- stale cache or stale data,
- serialization and schema compatibility,
- filesystem, database, or network boundaries,
- configuration and environment variables,
- concurrency,
- backward compatibility.

After listing assumptions, rank the top five by risk.

Do not generate test code yet.
``` 

## Expected output

```txt
Unit under test:
<short summary>

Hidden assumptions:
A1: ...
A2: ...
A3: ...

Top risk assumptions:
1. A3 — <reason>
2. A1 — <reason>
3. A2 — <reason>

Notes:
<uncertainties, missing context, existing tests that matter>
```
