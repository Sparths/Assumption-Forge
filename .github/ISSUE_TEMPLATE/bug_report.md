---
name: Bug report
description: Report incorrect, confusing, or low-quality guidance
title: "Bug: "
labels: ["bug"]
body:
  - type: markdown
    attributes:
      value: |
        Thanks for helping improve Assumption Forge.
  - type: textarea
    id: problem
    attributes:
      label: Problem
      description: What is incorrect, confusing, or harmful?
      placeholder: Describe the issue.
    validations:
      required: true
  - type: textarea
    id: location
    attributes:
      label: Location
      description: Which file, prompt, skill, or example is affected?
      placeholder: skills/assumption-forge.md
    validations:
      required: true
  - type: textarea
    id: expected
    attributes:
      label: Expected improvement
      description: What should be changed?
    validations:
      required: true
  - type: textarea
    id: context
    attributes:
      label: Additional context
      description: Add examples, links, or screenshots if useful. Do not include secrets or private code.
