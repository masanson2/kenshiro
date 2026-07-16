---
name: kenshiro-validate-impact
description: Validate and decide the mandatory Kenshiro impact approval gate.
version: 1.0.0
---

# Validate Impact

## Responsibility

Process only the impact approval command.

## Inputs

- `.kenshiro/state/workflow.yaml`
- `.kenshiro/state/impact.yaml`
- Exact command: `APPROVE IMPACT` or `REJECT IMPACT`
- `../shared/gates.yaml`

## Outputs

- Updated `.kenshiro/state/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require `current_phase: IMPACT_APPROVAL`. Approval sets `gates.impact.status: APPROVED` and `current_phase: GENERATE_TASKS`. Rejection sets `REJECTED` and `current_phase: IMPACT_ANALYSIS`. Do not generate tasks.

## State update

Persist the gate status and phase transition. Append `Impact Approved` or `Impact Rejected`.

## Completion criteria

Exactly one legal command is processed and the persisted workflow matches `shared/state-machine.yaml`.
