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
- `.kenshiro/state/feature.yaml`
- `.kenshiro/state/workflow.yaml`
- `../shared/schemas/impact.schema.yaml`

## Outputs

- `.kenshiro/state/impact.yaml`
- `.kenshiro/docs/impact.md`
- Updated `.kenshiro/state/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Inspect usages, callers, implementations, services, repositories, controllers, APIs, schedulers, batch jobs, database artifacts, tests, dependency chains, and potential regressions. Record findings only. Do not alter source code or propose implementation alternatives.

## State update

Set `skills.impact-analysis.status: DONE`, `gates.impact.status: PENDING`, and `current_phase: IMPACT_APPROVAL`. Append `Impact Analysis Completed`.

## Completion criteria

`impact.yaml` conforms to its schema, the Italian impact document exists, and no application files changed.
