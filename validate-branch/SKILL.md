---
name: kenshiro-validate-branch
description: Process branch approval or rename requests and create only the approved feature branch.
version: 1.0.0
---

# Validate Branch

## Responsibility

Process only branch approval and branch rename commands.

## Inputs

- `.kenshiro/features/<feature-id>/workflow.yaml`
- `.kenshiro/features/<feature-id>/git.yaml`
- `.kenshiro/features/<feature-id>/docs/branch.md`
- Exact command: `APPROVE BRANCH` or `RENAME BRANCH <new-name>`
- `../shared/gates.yaml`

## Outputs

- Updated `.kenshiro/features/<feature-id>/git.yaml`
- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require `current_phase: BRANCH_APPROVAL`, `repository: true`, and `feature_branch.status: PROPOSED`. Before `APPROVE BRANCH`, require `docs/branch.md` to be a complete Italian human-facing proposal with no unresolved placeholders and values matching `git.yaml`. `RENAME BRANCH <new-name>` accepts a non-empty Git branch name, changes only the technical proposal, regenerates `docs/branch.md`, keeps the branch gate `PENDING`, and does not create a branch. `APPROVE BRANCH` first verifies the proposed name does not already exist, then creates and checks out exactly that branch, then verifies the current branch equals the proposal. On success set `feature_branch.status: ACTIVE`, `feature_branch.current_branch` to the actual current branch, `git_branch.status: ACTIVE`, `gates.branch.status: APPROVED`, feature registry status `IN_PROGRESS`, and `current_phase: IMPACT_ANALYSIS`. On failure set `feature_branch.status: FAILED`, `git_branch.status: FAILED`, and `gates.branch.status: FAILED`.

## State update

Persist every command result. Append `Branch Renamed`, `Branch Approved`, or `Branch Creation Failed`.

## Completion criteria

For approval, exactly one branch exists for the feature and the current branch equals `git.yaml.feature_branch.name`. For rename, no Git branch is created.
