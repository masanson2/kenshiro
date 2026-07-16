---
name: kenshiro-review
description: Produce the concise human Kenshiro review document after validated implementation completion.
version: 1.0.0
---

# Review

## Responsibility

Create the human review artifact only.

## Inputs

- `.kenshiro/features/<feature-id>/workflow.yaml`
- `.kenshiro/features/<feature-id>/tasks.yaml`
- Implemented component list
- Executed validation and test results
- `../shared/templates/feature/docs/review.md`

## Outputs

- `.kenshiro/features/<feature-id>/docs/review.md`
- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Updated `.kenshiro/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require `current_phase: REVIEW` and all tasks `DONE` with `tdd.green: PASSED` and `compilation.status: PASSED`. Write concise Italian Markdown containing only modified components, implemented tasks, executed validations, executed tests, and remaining risks. Include the recorded quiet compilation and test commands with their results under executed validations. Never include reasoning, opinions, chain of thought, or unexecuted checks.

## State update

Set feature `skills.review.status: DONE` and `current_phase: DONE`. Set the feature registry entry to `COMPLETED`. Append `Review Completed`.

## Completion criteria

`review.md` exists with all five required sections and `workflow.yaml` is in the terminal `DONE` phase.
