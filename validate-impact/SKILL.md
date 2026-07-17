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
- `.kenshiro/features/<feature-id>/docs/impact.md`
- `.kenshiro/stack.yaml`
- Exact command: `APPROVE IMPACT` or `REJECT IMPACT`
- `../shared/gates.yaml`

## Outputs

- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require feature `current_phase: IMPACT_APPROVAL`. Approval is invalid unless `docs/impact.md` is a complete Italian technical and discursive analysis with no unresolved placeholders, a Mermaid class/component diagram, every impacted component's As-is and To-be state, all new or modified queries from `impact.yaml`, and the affected API contract. If `impact.yaml.api_contract.format` is not `NONE`, its path must equal `stack.yaml.api.source_of_truth.path` and every affected operation must be recorded. Its values must be consistent with `impact.yaml`. Approval sets `gates.impact.status: APPROVED` and `current_phase: GENERATE_TASKS`. Rejection sets `REJECTED` and `current_phase: IMPACT_ANALYSIS`. Do not create or propose branches.

## State update

Persist the gate status and phase transition. Append `Impact Approved` or `Impact Rejected`.

## Completion criteria

Exactly one legal command is processed and the persisted workflow matches `shared/state-machine.yaml`.
