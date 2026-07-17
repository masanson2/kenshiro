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
- `.kenshiro/features/<feature-id>/impact.yaml`
- `.kenshiro/features/<feature-id>/tasks.yaml`
- `.kenshiro/features/<feature-id>/docs/tasks.md`
- Exact command: `APPROVE TASKS` or `REJECT TASKS`
- `../shared/gates.yaml`

## Outputs

- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require feature `current_phase: TASK_APPROVAL`. Before approval, require `docs/tasks.md` to be a complete technical, discursive Italian plan with no unresolved placeholders, a Mermaid class/component diagram, query analysis, API contract analysis, As-is and To-be variations, and one entry for every task in `tasks.yaml`. When `impact.yaml.api_contract.format` is not `NONE`, require every relevant task scope to include the recorded source-of-truth contract path; otherwise reject the task gate and return to `GENERATE_TASKS`. Approval sets `gates.tasks.status: APPROVED`, `gates.implementation.status: PENDING`, and `current_phase: IMPLEMENTATION_APPROVAL`. Rejection sets `REJECTED` and `current_phase: GENERATE_TASKS`. Do not modify source code.

## State update

Persist the gate status and phase transition. Append `Tasks Approved` or `Tasks Rejected`.

## Completion criteria

Exactly one legal command is processed and the persisted workflow matches `shared/state-machine.yaml`.
