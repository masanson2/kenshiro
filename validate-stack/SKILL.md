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
- `.kenshiro/stack.yaml`
- `.kenshiro/workflow.yaml`
- Exact command: `APPROVE STACK` or `REJECT STACK`
- `../shared/gates.yaml`

## Outputs

- Updated `.kenshiro/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require root `project_phase: STACK_APPROVAL`. Approval is invalid if any mandatory `stack.yaml` field, including `build.compile` and `build.test`, is `UNKNOWN`, has an empty command, or has `quiet` other than `true`. `APPROVE STACK` sets root `project_gates.stack.status: APPROVED` and `project_phase: REQUIREMENTS_ANALYSIS`. `REJECT STACK` sets `REJECTED`, retains `project_phase: STACK_APPROVAL`, and stops workflow execution. Only `REFRESH STACK` authorizes a new stack analysis. No repository or requirements analysis occurs here.

## State update

Persist the gate status and phase transition. Append `Stack Approved` or `Stack Rejected`.

## Completion criteria

Exactly one legal command is processed and the persisted workflow matches `shared/state-machine.yaml`.
