---
name: kenshiro-validate-features
description: Process the mandatory feature approval gate before creating isolated feature workspaces.
version: 1.0.0
---

# Validate Features

## Responsibility

Process only the feature approval commands.

## Inputs

- `.kenshiro/workflow.yaml` with `project_phase: FEATURE_APPROVAL`
- `.kenshiro/features-analysis.md`
- Exact command: `APPROVE FEATURES` or `REJECT FEATURES`
- `../shared/gates.yaml`

## Outputs

- Updated `.kenshiro/workflow.yaml`
- `.kenshiro/features/<feature-id>/` only after approval
- Appended `.kenshiro/activity.log`

## Rules

`REJECT FEATURES` sets root `project_gates.features.status: REJECTED`, returns to `FEATURE_IDENTIFICATION`, and creates no directories, branches, tasks, or implementation. `APPROVE FEATURES` creates exactly one feature directory for each identified feature, initializes only `feature.yaml`, `impact.yaml`, `tasks.yaml`, `workflow.yaml`, `git.yaml`, and feature documents from the feature templates, registers every feature as `IN_PROGRESS`, and sets root `project_gates.features.status: APPROVED` and `project_phase: READY_FOR_FEATURE_WORK`. Assign each created feature an unused `YYYYMMDD-NNN-short-name` ID. Populate its `feature.yaml` from its approved capability and proposed branch, deriving `branch_type` from the approved branch prefix. Do not create or checkout a Git branch; branch creation remains exclusively in the feature's `BRANCH_APPROVAL` phase.

## State update

Append `Features Approved` or `Features Rejected`.

## Completion criteria

Exactly one directory exists per approved feature, no feature directory exists for a rejected proposal, and no Git branch was created by this gate.
