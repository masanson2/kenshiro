---
name: kenshiro-implementation
description: Implement only approved Kenshiro tasks while preserving deterministic per-task state.
version: 1.0.0
---

# Implementation

## Responsibility

Implement approved tasks only.

## Inputs

- `.kenshiro/project-index.yaml`
- `.kenshiro/features/<feature-id>/workflow.yaml`
- `.kenshiro/features/<feature-id>/feature.yaml`
- `.kenshiro/features/<feature-id>/impact.yaml`
- `.kenshiro/features/<feature-id>/tasks.yaml`
- `.kenshiro/features/<feature-id>/git.yaml`
- `../shared/implementation-guard.yaml`

## Outputs

- Application changes limited to approved task scope
- Updated `.kenshiro/features/<feature-id>/tasks.yaml`
- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Updated `.kenshiro/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Evaluate the implementation guard before any source change. Verify root `workflow.yaml.active_feature` equals `<feature-id>`. Verify feature gates `stack`, `impact`, `branch`, and `tasks` are `APPROVED`. Verify `.git` exists, `git.yaml.feature_branch.status: ACTIVE`, and the current Git branch equals `git.yaml.feature_branch.name`. If any check fails, set `git_branch.status: FAILED` in `git.yaml`, set the feature registry status to `BLOCKED`, append `Implementation Blocked`, and do not modify source code.

Never implement directly on `main`, `master`, `develop`, `release/*`, or `hotfix/*`. Treat a protected current branch as a failed branch check. Set `git_branch.status: FAILED`, `gates.branch.status: PENDING`, feature registry status `BLOCKED`, and `current_phase: PROPOSE_BRANCH`; stop without source changes. A new proposal, explicit `APPROVE BRANCH`, and branch creation are then required. Implement only `PENDING` tasks whose scopes match `impact.yaml`. Do not add dependencies unless a feature constraint explicitly permits it. Apply SOLID principles. Follow TDD using exactly `project-index.yaml.stack.build.test.command`: record `tdd.red: PASSED` after the relevant quiet test command fails, then implement the minimum production change and record `tdd.green: PASSED` after the same quiet test command passes.

After every source modification, execute exactly `project-index.yaml.stack.build.compile.command`. Execute no verbose compilation or test command. Do not infer, alter, replace, suppress externally, or skip either command. Persist the exact command and result in the task `compilation` object, append the activity event, and immediately stop the task with `compilation.status: FAILED` if compilation fails.

After a task's declared validation, TDD green phase, and latest compilation pass, create one local Git commit containing only that task's approved changes. Record its task ID, commit hash, and `CREATED` status in `git.yaml.commits`. If the commit fails, record `FAILED`, stop the task, and do not set it to `DONE`. `git.yaml.push` is always `FORBIDDEN`: never run `git push`, configure a push remote, create a pull request, or perform any remote publication. Set a task to `DONE` only after its local commit is recorded.

## State update

Persist task status after every task. When all tasks are `DONE`, set feature `skills.implementation.status: DONE` and `current_phase: REVIEW`. Append one activity event for each task status change.

## Completion criteria

All approved tasks are `DONE`, each declared validation has executed successfully, each task has `tdd.green: PASSED` and `compilation.status: PASSED`, and no modified component is outside approved scope.
