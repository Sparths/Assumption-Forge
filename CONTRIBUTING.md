# Contributing to Assumption Forge

Thank you for considering a contribution.

Assumption Forge is a public quality bar for AI-generated tests. Contributions should make the project clearer, more useful, or more rigorous.

## Good contributions

Useful contributions include:

- better examples in real languages,
- improved prompts,
- agent-specific integration guides,
- assumption taxonomies for specific domains,
- case studies where hidden-assumption tests caught real bugs,
- documentation improvements,
- quality checks for generated tests.

## Contribution standards

Before opening a pull request, make sure your change is:

- specific,
- practical,
- easy to read,
- not just marketing language,
- consistent with the existing structure,
- useful to people who want better tests from AI agents.

## Example contribution format

For new examples, use this structure:

```md
# <Language / Framework> Example

## Code under test

<small code sample>

## Hidden assumptions

A1: ...
A2: ...
A3: ...

## Example tests

<test code>

## Why these tests matter

<explanation>
```

## Prompt contribution checklist

When adding or changing a prompt, check:

- Does it force assumption extraction before test generation?
- Does it ask for evidence?
- Does it rank risk?
- Does it discourage fake confidence?
- Does it produce reviewable output?
- Does it work without relying on one specific AI vendor?

## Pull request checklist

Include this in your PR description:

```txt
- [ ] This improves test quality, agent behavior, or project clarity.
- [ ] This avoids vague AI-generated filler.
- [ ] Examples include hidden assumptions and why the tests matter.
- [ ] Documentation is clear for public readers.
- [ ] I have checked spelling and Markdown formatting.
```

## Style

Use clear English. Prefer concrete examples over abstract claims.

Avoid saying a method is "perfect", "guaranteed", or "fully automated". Assumption Forge improves test generation, but humans should still review requirements and product intent.

## Code of conduct

Be respectful and constructive. See `CODE_OF_CONDUCT.md`.
