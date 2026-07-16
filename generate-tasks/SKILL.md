---
name: kenshiro-generate-tasks
description: Convert approved Kenshiro impact findings into minimal verifiable implementation tasks.
version: 1.0.0
---

# Generate Tasks

## Responsibility

Convert approved impact into tasks only.

## Inputs

- `.kenshiro/project-index.yaml`
- `.kenshiro/features/<feature-id>/feature.yaml`
- `.kenshiro/features/<feature-id>/impact.yaml`
- `.kenshiro/features/<feature-id>/git.yaml`
- `.kenshiro/features/<feature-id>/workflow.yaml` with approved impact and branch gates
- `../shared/schemas/tasks.schema.yaml`

## Outputs

- `.kenshiro/features/<feature-id>/tasks.yaml`
- `.kenshiro/features/<feature-id>/docs/tasks.md`
- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Each task has an ID, compact English description, `PENDING` status, scope, validation, `tdd: {red: PENDING, green: PENDING, command: <project-index.stack.build.test.command>}`, and `compilation: {status: PENDING, command: <project-index.stack.build.compile.command>}`. Copy both quiet commands byte-for-byte from approved project state. Tasks must be independent, minimal, verifiable, within approved impact, and designed with tests before production changes. Do not modify source code.

## State update

Set feature `skills.generate-tasks.status: DONE`, `gates.tasks.status: PENDING`, and `current_phase: TASK_APPROVAL`. Append `Generate Tasks Completed`.

## Completion criteria

`tasks.yaml` conforms to its schema and every task scope is present in `impact.yaml`.
