# Assumption Forge

**Forge tests from hidden assumptions.**

Assumption Forge is an open-source testing skill for AI coding agents. It teaches agents to stop writing shallow, coverage-chasing tests and instead discover the implicit assumptions inside code, then generate tests that deliberately challenge those assumptions.

Most AI-generated tests confirm the happy path. Assumption Forge focuses on the paths nobody wrote down.

> What must be true for this code to work, even though the code never says it?

That question is the core of the project.

---

## Why this exists

AI coding tools are getting good at producing tests that look plausible. But plausible tests often create fake confidence:

- they mirror the implementation instead of validating behavior,
- they overfit to the current output,
- they mock away the risk,
- they miss invalid states and boundary conditions,
- they increase coverage without increasing trust.

Assumption Forge gives agents a stricter testing protocol. Before writing code, the agent must extract hidden assumptions, rank their risk, choose the right test type, and only then generate tests.

The goal is not more tests.

The goal is **tests that would fail for the right reason**.

---

## What Assumption Forge does

Assumption Forge is currently a skill, prompt framework, and methodology. It can be used with AI coding agents such as Claude Code, Cursor, Codex-style agents, GitHub Copilot Chat, Aider, Cline, Roo, or any assistant that can read project instructions.

It helps agents:

- identify hidden assumptions in code,
- convert assumptions into concrete test cases,
- prioritize high-risk behavior,
- avoid shallow assertion patterns,
- generate maintainable tests in the project's existing style,
- explain why each test matters,
- self-review tests before presenting them.

---

## Quick start

Copy the core skill into your agent's instruction file:

```bash
cp skills/assumption-forge.md ./ASSUMPTION_FORGE.md
```

Then tell your agent:

```txt
Use ASSUMPTION_FORGE.md. Analyze this module and generate assumption-breaking tests.
```

For tools that support persistent rules, copy the relevant adapter or paste the skill into your agent configuration.

Suggested locations:

| Agent / Tool | Suggested location |
|---|---|
| Claude Code | `CLAUDE.md` or project memory |
| Cursor | `.cursor/rules/assumption-forge.mdc` |
| Codex-style agents | `AGENTS.md` or project instructions |
| GitHub Copilot Chat | repository instructions or prompt file |
| Aider | `.aider.conf.yml` / conventions document |
| Cline / Roo | custom instructions |

---

## Example workflow

Ask your agent:

```txt
Use Assumption Forge on src/billing/applyDiscount.ts.
Do not write tests immediately.
First extract hidden assumptions, rank them by risk, then generate the smallest useful test set.
```

Expected shape of the agent's answer:

```txt
Unit under test:
applyDiscount(amount, discount, currency)

Hidden assumptions:
A1: amount is always a finite decimal value.
Risk: NaN, Infinity, and floating point precision can corrupt billing totals.
Priority: High
Test idea: reject non-finite values and verify decimal-safe rounding.

A2: discounts never exceed the original amount.
Risk: negative charges or credits may be generated accidentally.
Priority: High
Test idea: test 100%, over-100%, and negative discount values.

Test plan:
1. Reject non-finite amount values.
2. Preserve exact cents for decimal inputs.
3. Reject discounts greater than the original amount.
4. Handle zero discount without changing amount.
5. Verify currency-specific minor units.
```

Then the agent writes tests that target those assumptions directly.

---

## Repository structure

```txt
.
├── README.md
├── skills/
│   ├── assumption-forge.md
│   └── assumption-forge.compact.md
├── prompts/
│   ├── assumption-extraction.md
│   ├── test-plan.md
│   ├── test-generation.md
│   ├── self-review.md
│   └── regression-from-bug.md
├── docs/
│   ├── principles.md
│   ├── assumption-taxonomy.md
│   └── agent-integration.md
├── examples/
│   ├── python/
│   ├── typescript/
│   └── rust/
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
└── LICENSE
```

---

## Core idea

Assumption Forge changes the order of test generation.

Most agents do this:

```txt
read code → write tests → explain tests
```

Assumption Forge requires this:

```txt
read code → extract assumptions → rank risk → choose test types → write tests → self-review
```

This matters because the highest-value tests usually come from unstated assumptions, not from visible branches.

---

## What counts as a hidden assumption?

A hidden assumption is anything the code relies on without explicitly enforcing, documenting, or testing.

Examples:

- IDs are unique.
- Dates are in UTC.
- Money has exactly two decimal places.
- A list is already sorted.
- A dependency always returns complete data.
- Retrying an operation is safe.
- A user who is authenticated is also authorized.
- A file path is local and normalized.
- Cache data is fresh.
- JSON fields exist and have the expected type.
- The database query returns rows in stable order.
- A failed external call leaves no partial state behind.

Assumption Forge turns these into tests.

---

## What makes an Assumption Forge test good?

A good test:

- protects against a named assumption,
- has a clear failure mode,
- asserts observable behavior,
- uses minimal mocking,
- follows the project's existing style,
- is deterministic,
- would fail if the implementation were subtly wrong,
- is small enough to understand,
- explains the risk it protects against.

A weak test:

- only checks that a function returns something,
- duplicates the implementation formula,
- snapshots a huge object without intent,
- mocks the behavior under test,
- asserts internal details unnecessarily,
- exists only to raise coverage.

---

## Prompt library

The `prompts/` directory contains reusable prompts for different parts of the workflow:

- `assumption-extraction.md` — find hidden assumptions before writing tests.
- `test-plan.md` — convert assumptions into a prioritized test plan.
- `test-generation.md` — write the tests in the project's existing style.
- `self-review.md` — audit generated tests for fake confidence.
- `regression-from-bug.md` — convert a bug report into regression tests.

You can use them independently or together.

---

## Language examples

The `examples/` directory includes practical examples for:

- Python with pytest,
- TypeScript with Vitest/Jest-style tests,
- Rust with cargo test.

These examples are intentionally small. The important part is the reasoning pattern, not the framework.

---

## Design principles

Assumption Forge follows these principles:

1. **Risk before coverage** — coverage is a signal, not the goal.
2. **Behavior before implementation** — test what users and callers can observe.
3. **Assumptions before assertions** — know what you are protecting before asserting.
4. **Smallest reliable test** — choose the narrowest test that exposes the risk.
5. **No fake confidence** — passing tests should mean something.
6. **Explain the failure mode** — every test should have a reason to exist.

See [`docs/principles.md`](docs/principles.md) for the full testing philosophy.

---

## Roadmap

Assumption Forge starts as a skill and prompt framework. Possible future additions:

- CLI for scanning code and producing assumption inventories,
- adapters for common AI coding agents,
- JSON schema for assumption reports,
- GitHub Action for reviewing test quality in pull requests,
- benchmark suite for comparing AI-generated tests,
- mutation-testing integration,
- property-based testing recipes,
- language-specific assumption taxonomies.

---

## Contributing

Contributions are welcome, especially:

- better test-generation examples,
- language-specific guidance,
- real-world assumption taxonomies,
- prompts for additional agents,
- case studies where assumption-based tests found real bugs.

Read [`CONTRIBUTING.md`](CONTRIBUTING.md) before opening a pull request.

---

## License

MIT. See [`LICENSE`](LICENSE).
