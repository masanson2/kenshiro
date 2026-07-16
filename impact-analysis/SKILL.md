---
name: kenshiro-impact-analysis
description: Find repository-wide feature impacts and dependency chains without modifying source code.
version: 1.0.0
---

# Impact Analysis

## Responsibility

Perform usage and dependency analysis only.

## Inputs

- Target repository
- `.kenshiro/features/<feature-id>/feature.yaml`
- `.kenshiro/features/<feature-id>/workflow.yaml`
- `../shared/schemas/impact.schema.yaml`

## Outputs

- `.kenshiro/features/<feature-id>/impact.yaml`
- `.kenshiro/features/<feature-id>/docs/impact.md`
- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Inspect usages, callers, implementations, services, repositories, controllers, APIs, schedulers, batch jobs, database artifacts, tests, dependency chains, and potential regressions for this feature only. Record findings only. Do not combine impacts with another feature, alter source code, or propose implementation alternatives.

## State update

Set feature `skills.impact-analysis.status: DONE`, `gates.impact.status: PENDING`, and `current_phase: IMPACT_APPROVAL`. Append `Impact Analysis Completed`.

## Completion criteria

`impact.yaml` conforms to its schema, the Italian impact document exists, and no application files changed.
