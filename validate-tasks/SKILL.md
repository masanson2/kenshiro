---
name: kenshiro-validate-tasks
description: Validate and decide the mandatory Kenshiro task approval gate.
version: 1.0.0
---

# Validate Tasks

## Responsibility

Process only the task approval command.

## Inputs

- `.kenshiro/features/<feature-id>/workflow.yaml`
- `.kenshiro/features/<feature-id>/tasks.yaml`
- Exact command: `APPROVE TASKS` or `REJECT TASKS`
- `../shared/gates.yaml`

## Outputs

- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require feature `current_phase: TASK_APPROVAL`. Approval sets `gates.tasks.status: APPROVED` and `current_phase: IMPLEMENTATION`. Rejection sets `REJECTED` and `current_phase: GENERATE_TASKS`. Do not modify source code.

## State update

Persist the gate status and phase transition. Append `Tasks Approved` or `Tasks Rejected`.

## Completion criteria

Exactly one legal command is processed and the persisted workflow matches `shared/state-machine.yaml`.
