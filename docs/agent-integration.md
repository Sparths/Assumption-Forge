# Agent Integration Guide

Assumption Forge is intentionally tool-agnostic. Any AI coding assistant can use it if it can read instructions.

## General integration

1. Copy `skills/assumption-forge.md` into your project.
2. Reference it from your agent instructions.
3. Ask the agent to extract assumptions before writing tests.
4. Review the assumption inventory before accepting generated tests.

Recommended prompt:

```txt
Use Assumption Forge.
Analyze the target code, extract hidden assumptions, rank them by risk, create a test plan, then write tests that challenge the highest-risk assumptions.
```

## Claude Code

Suggested file:

```txt
CLAUDE.md
```

Suggested content:

```txt
When asked to write tests, use the Assumption Forge workflow from skills/assumption-forge.md. Do not write tests before extracting hidden assumptions and producing a test plan.
```

## Cursor

Suggested file:

```txt
.cursor/rules/assumption-forge.mdc
```

Suggested content:

```txt
---
description: Generate tests from hidden assumptions
alwaysApply: false
---

Use the Assumption Forge workflow. Before writing tests, identify hidden assumptions, rank risk, produce a test plan, then generate tests with specific assertions and minimal mocking.
```

## Codex-style agents

Suggested file:

```txt
AGENTS.md
```

Suggested content:

```txt
For testing tasks, follow skills/assumption-forge.md. Prefer assumption inventories and risk-ranked test plans before writing test code.
```

## GitHub Copilot Chat

Store the compact skill in your repository instructions or paste it into the chat before asking for tests.

Suggested prompt:

```txt
Use this testing method: extract hidden assumptions first, then generate tests. Do not optimize for line coverage alone.
```

## Cline / Roo / Aider

Use the project instructions or custom rules feature. If context is limited, use `skills/assumption-forge.compact.md`.

## Review guidance for humans

When reviewing AI-generated tests, ask:

- Which assumption does this test protect?
- Would this test fail for the wrong implementation?
- Is the assertion specific?
- Is the setup smaller than the behavior being tested?
- Did the agent invent requirements?
- Are important risks still untested?

If the answer to the first question is unclear, request a revision.
