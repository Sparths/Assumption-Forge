# TypeScript Example — Vitest

TypeScript types help during development, but runtime boundaries still receive unknown data from APIs, forms, storage, URLs, and third-party packages.

## Code under test

```ts
type User = {
  id: string;
  email: string;
  role: "admin" | "member";
};

export function indexUsersById(users: User[]): Record<string, User> {
  return Object.fromEntries(users.map((user) => [user.id, user]));
}
```

## Hidden assumptions

```txt
Assumption A1: User IDs are unique.
Evidence: Object.fromEntries overwrites earlier entries with the same key.
Risk: Duplicate IDs silently drop users.
Test idea: Provide duplicate IDs and assert explicit behavior.
Priority: High

Assumption A2: User IDs are non-empty strings.
Evidence: No validation before using id as object key.
Risk: Empty IDs create ambiguous lookup behavior.
Test idea: Provide empty and whitespace IDs.
Priority: Medium

Assumption A3: Runtime input always matches the TypeScript type.
Evidence: Function accepts typed input, but callers may pass parsed JSON.
Risk: malformed runtime data creates incorrect index.
Test idea: Add validation at boundary or test caller that performs validation.
Priority: Medium
```

## Example tests

```ts
import { describe, expect, it } from "vitest";
import { indexUsersById } from "./users";

describe("indexUsersById", () => {
  it("preserves lookup by unique user id", () => {
    const users = [
      { id: "u_1", email: "a@example.com", role: "member" as const },
      { id: "u_2", email: "b@example.com", role: "admin" as const },
    ];

    expect(indexUsersById(users)).toEqual({
      u_1: users[0],
      u_2: users[1],
    });
  });

  it("does not silently overwrite duplicate user ids", () => {
    const users = [
      { id: "u_1", email: "first@example.com", role: "member" as const },
      { id: "u_1", email: "second@example.com", role: "admin" as const },
    ];

    expect(() => indexUsersById(users)).toThrow(/duplicate/i);
  });

  it("rejects empty user ids", () => {
    const users = [
      { id: "", email: "empty@example.com", role: "member" as const },
    ];

    expect(() => indexUsersById(users)).toThrow(/id/i);
  });
});
```

## Why these tests matter

A shallow test would only verify that two valid users become two keys. Assumption Forge adds tests for the assumptions that can silently corrupt the index: duplicate IDs and invalid IDs.

The implementation may need to change to satisfy these tests. That is the point: the tests reveal an implicit contract that should be made explicit.
