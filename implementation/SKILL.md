---
name: kenshiro-implementation
description: Implement only approved Kenshiro tasks while preserving deterministic per-task state.
version: 1.0.0
---

# Implementation

## Responsibility

Implement approved tasks only.

## Inputs

- `.kenshiro/state/workflow.yaml`
- `.kenshiro/state/stack.yaml`
- `.kenshiro/state/feature.yaml`
- `.kenshiro/state/impact.yaml`
- `.kenshiro/state/tasks.yaml`
- `../shared/implementation-guard.yaml`

## Outputs

- Application changes limited to approved task scope
- Updated `.kenshiro/state/tasks.yaml`
- Updated `.kenshiro/state/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Evaluate the implementation guard before any source change. Read all input state files. Implement only `PENDING` tasks whose scopes match `impact.yaml`. Do not add dependencies unless a feature constraint explicitly permits it. Update a task to `IN_PROGRESS` before changing code and to `DONE` only after its declared validation succeeds.

## State update

Persist task status after every task. When all tasks are `DONE`, set `skills.implementation.status: DONE` and `current_phase: REVIEW`. Append one activity event for each task status change.

## Completion criteria

All approved tasks are `DONE`, each declared validation has executed successfully, and no modified component is outside approved scope.
