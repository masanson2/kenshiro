---
name: kenshiro-requirement-decomposition
description: Decompose analyzed requirements into independent business capabilities before feature identification.
version: 1.0.0
---

# Requirement Decomposition

## Responsibility

Determine whether analyzed requirements contain one business capability or multiple independent business capabilities.

## Inputs

- `.kenshiro/workflow.yaml` with `project_phase: REQUIREMENT_DECOMPOSITION`
- User-provided change request

## Outputs

- Updated `.kenshiro/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Treat different business domains, APIs, bounded contexts, workflows, and independently deployable behaviors as independent capabilities. When uncertain, decompose rather than group. Record each capability in root `workflow.yaml.capabilities` with a name, description, reason for separation, and a `feat/<short-name>` proposed branch. Do not create a feature directory, Git branch, task, impact analysis, or implementation.

## State update

Set root `project_skills.requirement-decomposition.status: DONE` and `project_phase: FEATURE_IDENTIFICATION`. Append `Requirement Decomposition Completed`.

## Completion criteria

Every requested behavior belongs to exactly one capability, and no independent capability has been merged with another.
