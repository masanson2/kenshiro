---
name: kenshiro-validate-stack
description: Validate and decide the mandatory Kenshiro stack approval gate.
version: 1.0.0
---

# Validate Stack

## Responsibility

Process only the stack approval command.

## Inputs

- `.kenshiro/project-index.yaml`
- `.kenshiro/features/<feature-id>/workflow.yaml`
- Exact command: `APPROVE STACK` or `REJECT STACK`
- `../shared/gates.yaml`

## Outputs

- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require the active feature `current_phase: STACK_APPROVAL`. Approval is invalid if any mandatory project-index stack field, including `build.compile`, is `UNKNOWN` or its command is empty. `APPROVE STACK` sets feature `gates.stack.status: APPROVED` and `current_phase: ANALYZE_SPECIFICATION`. `REJECT STACK` sets `REJECTED` and `current_phase: ANALYZE_PROJECT`. No repository analysis or specification analysis occurs here.

## State update

Persist the gate status and phase transition. Append `Stack Approved` or `Stack Rejected`.

## Completion criteria

Exactly one legal command is processed and the persisted workflow matches `shared/state-machine.yaml`.
