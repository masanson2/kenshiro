---
name: kenshiro-propose-branch
description: Detect Git and produce exactly one feature-to-branch proposal without creating a branch.
version: 1.0.0
---

# Propose Branch

## Responsibility

Detect Git and create one branch proposal for the active feature.

## Inputs

- Target repository `.git`
- `.kenshiro/project-index.yaml`
- `.kenshiro/features/<feature-id>/feature.yaml`
- `.kenshiro/features/<feature-id>/workflow.yaml`
- `../shared/schemas/git.schema.yaml`
- `../shared/templates/feature/docs/branch.md`

## Outputs

- `.kenshiro/features/<feature-id>/git.yaml`
- `.kenshiro/features/<feature-id>/docs/branch.md`
- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require `current_phase: PROPOSE_BRANCH`. Detect Git by checking `.git`. If absent, write `repository: false`, set feature `gates.branch.status: FAILED`, and stop. Require `feature.proposed_branch` from the approved feature analysis; if it is empty or does not match its `branch_type`, set `gates.branch.status: FAILED` and stop. If present, read the repository base branch and propose exactly that one approved branch name. Do not create, checkout, or modify a branch.

## State update

For Git repositories, write the technical branch proposal only to `git.yaml`. Render `docs/branch.md` as a concise Italian human-facing proposal stating the base branch, proposed branch, and that no branch has yet been created. Set `skills.propose-branch.status: DONE`, `gates.branch.status: PENDING`, and `current_phase: BRANCH_APPROVAL`. Append `Branch Proposed`.

## Completion criteria

`git.yaml` conforms to its schema, `docs/branch.md` exists in Italian with no unresolved placeholders, exactly one proposal exists for the feature, and no Git branch was created.
