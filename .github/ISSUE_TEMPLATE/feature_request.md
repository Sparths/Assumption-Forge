---
name: Feature request
description: Suggest a prompt, skill improvement, example, or integration
title: "Feature: "
labels: ["enhancement"]
body:
  - type: markdown
    attributes:
      value: |
        Suggest a concrete improvement to Assumption Forge.
  - type: textarea
    id: proposal
    attributes:
      label: Proposal
      description: What should be added or changed?
      placeholder: Add a new example for Go table-driven tests.
    validations:
      required: true
  - type: textarea
    id: problem
    attributes:
      label: Problem it solves
      description: What testing or agent-quality problem does this address?
    validations:
      required: true
  - type: textarea
    id: quality
    attributes:
      label: Assumption Forge quality angle
      description: How does this help agents write better assumption-driven tests?
    validations:
      required: true
  - type: textarea
    id: alternatives
    attributes:
      label: Alternatives considered
      description: Optional. Any other approaches?
