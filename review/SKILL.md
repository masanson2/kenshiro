---
name: kenshiro-review
description: Produce the concise human Kenshiro review document after deterministic implementation completion.
version: 1.0.0
---

# Review

## Responsibility

Create the human review artifact only.

## Inputs

- `.kenshiro/state/workflow.yaml`
- `.kenshiro/state/tasks.yaml`
- Implemented component list
- Executed validation and test results
- `../shared/templates/docs/review.md`

## Outputs

- `.kenshiro/docs/review.md`
- Updated `.kenshiro/state/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require `current_phase: REVIEW` and all tasks `DONE` with `tdd.green: PASSED` and `compilation.status: PASSED`. Write concise Italian Markdown containing only modified components, implemented tasks, executed validations, executed tests, and remaining risks. Include the recorded compilation command and result under executed validations. Never include reasoning, opinions, chain of thought, or unexecuted checks.

## State update

Set `skills.review.status: DONE` and `current_phase: DONE`. Append `Review Completed`.

## Completion criteria

`review.md` exists with all five required sections and `workflow.yaml` is in the terminal `DONE` phase.
