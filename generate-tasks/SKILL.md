---
name: kenshiro-generate-tasks
description: Convert approved Kenshiro impact findings into minimal verifiable implementation tasks.
version: 1.0.0
---

# Generate Tasks

## Responsibility

Convert approved impact into tasks only.

## Inputs

- `.kenshiro/state/feature.yaml`
- `.kenshiro/state/impact.yaml`
- `.kenshiro/state/workflow.yaml` with approved impact gate
- `../shared/schemas/tasks.schema.yaml`

## Outputs

- `.kenshiro/state/tasks.yaml`
- `.kenshiro/docs/tasks.md`
- Updated `.kenshiro/state/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Each task has an ID, compact English description, `PENDING` status, scope, and validation. Tasks must be independent, minimal, verifiable, and within approved impact. Do not modify source code.

## State update

Set `skills.generate-tasks.status: DONE`, `gates.tasks.status: PENDING`, and `current_phase: TASK_APPROVAL`. Append `Generate Tasks Completed`.

## Completion criteria

`tasks.yaml` conforms to its schema and every task scope is present in `impact.yaml`.
