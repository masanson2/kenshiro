---
name: kenshiro-validate-impact
description: Validate and decide the mandatory Kenshiro impact approval gate.
version: 1.0.0
---

# Validate Impact

## Responsibility

Process only the impact approval command.

## Inputs

- `.kenshiro/features/<feature-id>/workflow.yaml`
- `.kenshiro/features/<feature-id>/impact.yaml`
- Exact command: `APPROVE IMPACT` or `REJECT IMPACT`
- `../shared/gates.yaml`

## Outputs

- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require feature `current_phase: IMPACT_APPROVAL`. Approval sets `gates.impact.status: APPROVED` and `current_phase: PROPOSE_BRANCH`. Rejection sets `REJECTED` and `current_phase: IMPACT_ANALYSIS`. Do not create or propose branches.

## State update

Persist the gate status and phase transition. Append `Impact Approved` or `Impact Rejected`.

## Completion criteria

Exactly one legal command is processed and the persisted workflow matches `shared/state-machine.yaml`.
